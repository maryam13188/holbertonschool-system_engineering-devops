# Secured and Monitored Web Infrastructure

## Diagram

```mermaid
graph TD
    User((User)) -->|"https://www.foobar.com"| DNS[DNS Server]
    DNS -->|"8.8.8.8"| FW1["Firewall 1 + HAProxy"]

    FW1 -->|"Round Robin"| FW2["Firewall 2 - Server 1"]
    FW1 -->|"Round Robin"| FW3["Firewall 3 - Server 2"]

    subgraph S1 ["Server 1"]
        Web1[Nginx]
        App1[App Server]
        Code1[Codebase]
        DB1[(MySQL Master)]
        Mon1[Monitoring Agent]
    end

    subgraph S2 ["Server 2"]
        Web2[Nginx]
        App2[App Server]
        Code2[Codebase]
        DB2[(MySQL Replica)]
        Mon2[Monitoring Agent]
    end

    subgraph S3 ["Server 3"]
        Mon3[Central Collector]
    end

    FW1 -->|"HTTPS"| Web1
    FW1 -->|"HTTPS"| Web2

    Web1 --> App1
    App1 -->|"Write"| DB1

    DB1 -->|"Replication"| DB2

    Web2 --> App2
    App2 -->|"Read"| DB2

    Mon1 -->|"Logs / Metrics"| Mon3
    Mon2 -->|"Logs / Metrics"| Mon3
```

## Questions & Answers

| Question | Answer |
|-----------|-----------|
| Why add 3 firewalls? | To protect the network at multiple layers. One firewall is placed in front of the load balancer, and the others protect the servers from unauthorized access. |
| Why HTTPS? | To encrypt traffic between users and servers, protecting sensitive data from eavesdropping and tampering. |
| Why 3 monitoring clients? | To collect performance metrics and logs from each server and send them to a centralized monitoring service. |
| How does monitoring collect data? | Monitoring agents run on each server, gather logs and system metrics, then forward them to a central collector. |
| How to monitor QPS? | By analyzing Nginx access logs and counting the number of requests received per second. |
| Issue: SSL termination at LB? | If HTTPS is terminated at the load balancer and traffic is forwarded internally using HTTP, internal communications are not encrypted. |
| Issue: Single Master for writes? | If the Master database fails, write operations cannot be performed until failover occurs. |
| Issue: All components on same servers? | Sharing CPU, memory, and disk resources can create performance bottlenecks. Separating services improves scalability and reliability. |
