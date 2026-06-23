# Web Infrastructure Design

## Diagram

```mermaid
graph TD
    User((User)) -->|"www.foobar.com"| DNS[DNS Server]
    DNS -->|"8.8.8.8"| LB_Cluster[Load Balancer Cluster]

    subgraph HACluster ["HAProxy Cluster"]
        LB1["HAProxy 1 (Active)"]
        LB2["HAProxy 2 (Active)"]
    end

    LB1 -->|"Round Robin"| Web_Server
    LB2 -->|"Round Robin"| Web_Server

    LB1 -->|"Round Robin"| App_Server
    LB2 -->|"Round Robin"| App_Server

    subgraph Web_Server ["Server 1 - 8.8.8.8"]
        Web[Nginx]
    end

    subgraph App_Server ["Server 2 - 8.8.8.9"]
        App["Application Server + Codebase"]
    end

    subgraph DB_Server ["Server 3 - 8.8.8.10"]
        DB[(MySQL Database)]
    end

    Web -->|"Dynamic Requests"| App
    App -->|"Read / Write"| DB

    LB1 <-->|"Cluster Sync"| LB2
```

## Questions & Answers

### Why add a 3rd server?
To dedicate a server solely to the database. This isolates I/O-intensive database operations from the web and application layers, reducing resource contention and improving performance.

### Why add a 2nd load balancer?
To create a High Availability (HA) cluster with the first load balancer. This removes the load balancer as a Single Point of Failure (SPOF). If one load balancer fails, the other continues serving traffic.

### Why split components (Web, App, DB) onto separate servers?

1. **Resource Optimization**  
   Each component has different resource requirements:
   - Web Server → Network I/O
   - Application Server → CPU and Memory
   - Database Server → Disk I/O

2. **Security Isolation**  
   Compromising one layer does not automatically expose the others, especially the database layer.

3. **Easier Maintenance**  
   Each tier can be upgraded, restarted, or maintained independently.

4. **Better Scalability**  
   Additional web or application servers can be added without modifying the database layer.
