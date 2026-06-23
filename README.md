# Web Infrastructure Design

This project contains diagrams and explanations for designing scalable, secure, and monitored web infrastructures for the website `www.foobar.com`.

## Tasks Overview

### 0. Simple Web Stack
- **Components**: 1 server, 1 web server (Nginx), 1 application server, 1 codebase, 1 MySQL database.
- **Purpose**: Basic LAMP-like architecture to understand core components and their roles.
- **Diagram**: `images/simple_web_stack.png`

### 1. Distributed Web Infrastructure
- **Components**: 1 load balancer (HAproxy), 2 servers (each with Nginx, app server, codebase), and a MySQL Master-Replica setup.
- **Purpose**: Introduce redundancy and load distribution to handle more traffic.
- **Diagram**: `images/distributed_web_infrastructure.png`

### 2. Secured and Monitored Web Infrastructure
- **Components**: 3 firewalls, 1 SSL certificate (HTTPS), 3 monitoring clients (Sumologic), plus the distributed setup.
- **Purpose**: Add security layers and active monitoring to ensure data protection and early failure detection.
- **Diagram**: `images/secured_and_monitored_web_infrastructure.png`

### 3. Scale Up
- **Components**: 1 additional server, 1 additional load balancer (HAproxy cluster), and fully separated components (Web, App, DB on dedicated servers).
- **Purpose**: Eliminate Single Point of Failure (SPOF) and optimize resource usage by isolating tiers.
- **Diagram**: `images/scale_up.png`

## Technologies Used
- **Web Server**: Nginx
- **Application Server**: (Generic, runs the codebase)
- **Database**: MySQL
- **Load Balancer**: HAproxy
- **Firewall**: (Generic)
- **Monitoring**: Sumologic (or similar)
- **Security**: SSL/TLS (HTTPS)

## Learning Objectives
- Understand the role of each component in a web stack.
- Explain system redundancy, high availability, and scalability.
- Identify SPOFs, security risks, and performance bottlenecks.

