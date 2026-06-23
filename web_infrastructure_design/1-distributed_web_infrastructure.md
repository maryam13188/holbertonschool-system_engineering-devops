# Web Infrastructure Design

## Diagram

```mermaid
graph TD
    User((User)) -->|"www.foobar.com"| DNS[DNS Server]
    DNS -->|"8.8.8.8"| LB["Load Balancer (HAProxy)"]

    LB -->|"Round Robin"| Server1["Server 1 (8.8.8.8)"]
    LB -->|"Round Robin"| Server2["Server 2 (8.8.8.9)"]

    subgraph Server1 ["Server 1"]
        Web1[Nginx]
        App1[App Server]
        Code1[Codebase]
        DB1[(MySQL Master)]
    end

    subgraph Server2 ["Server 2"]
        Web2[Nginx]
        App2[App Server]
        Code2[Codebase]
        DB2[(MySQL Replica)]
    end

    Web1 --> App1
    App1 -->|"Write"| DB1
    DB1 -->|"Replication"| DB2
    Web2 --> App2
    App2 -->|"Read"| DB2
```

## Questions & Answers

### Why add a load balancer?
To distribute incoming traffic across multiple servers, reducing load on any single server and improving reliability.

### Why add a second server?
For redundancy – if one server fails, the other continues serving traffic.

### Why add a database replica?
To separate reads from writes, improve performance, and provide a backup.

### Load balancer algorithm?
Round Robin – requests are cycled sequentially (1st → Server1, 2nd → Server2, etc.).

### Active-Active or Active-Passive?
Active-Active – both servers handle live traffic simultaneously.

### How does Master-Replica work?
Master handles all writes (INSERT/UPDATE/DELETE) and replicates data to the Replica, which serves reads (SELECT).

### Difference for the application?
Application writes to Master and reads from Replica.

### Remaining issues?
- SPOF still exists (load balancer or Master DB).
- No firewall – insecure.
- No HTTPS – unencrypted traffic.
- No monitoring – can't detect failures early.
