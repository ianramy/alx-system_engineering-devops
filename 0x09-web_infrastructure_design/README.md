# Web Infrastructure Design – ALX

This repository explores fundamental and advanced web infrastructure concepts, progressively building up from a simple server to a secure, distributed, and scalable architecture.

## Tasks Overview

### Task File Description

0 0-simple_web_stack Basic single-server web infrastructure
1 1-distributed_web_infrastructure Introduces load balancing and distribution
2 2-secured_and_monitored_web_infrastructure Adds firewalls, HTTPS, and monitoring
3 3-scale_up Scales components and introduces clustered load balancers

## Task 0: Simple Web Stack

### File: 0-simple_web_stack

#### Architecture

```rust
User -> DNS (A Record) -> Server (8.8.8.8)
                           ├─ Nginx (Web Server)
                           ├─ App Server (Code)
                           └─ MySQL (Database)
```

## Key Concepts

Server: Single machine hosting all services
Domain Name: foobar.com with A record www pointing to 8.8.8.8
Nginx: Handles HTTP, static files, reverse proxy
Application Server: Executes logic (e.g., PHP, Flask)
MySQL: Manages persistent data

### Task 0 Issues

Single Point of Failure (SPOF)
Downtime during deployments
No scaling capability

## Task 1: Distributed Web Infrastructure

### File: 1-distributed_web_infrastructure

```rust
User -> DNS -> Load Balancer (HAProxy)
                         ├─ Web/App Server #1
                         └─ Web/App Server #2
                               └─ MySQL Database
```

### Additions

Load Balancer (HAProxy): Splits traffic using algorithms like Round Robin
Two App Servers: Increases availability
MySQL (Primary only): Centralized data store

### Concepts

Load Balancing: Evenly distributes requests
Active-Active Setup: All servers actively handle traffic
Primary-Replica DB: (Optionally mentioned) Read/write splitting

### Task 1 Issues

SPOF in DB
No HTTPS or firewall
No monitoring tools

## Task 2: Secured and Monitored Infrastructure

### File: 2-secured_and_monitored_web_infrastructure

```rust
User -> DNS -> Firewall #1 -> Load Balancer (HTTPS)
                          -> Firewall #2 -> Web/App Servers
                                           -> Firewall #3 -> MySQL
```

Every server includes monitoring agents

## Security & Monitoring

### 3 Firewalls: One each for Load Balancer, App Layer, and Database

HTTPS via SSL Cert: Encrypts user traffic
Monitoring Agents: SumoLogic or Prometheus collect metrics/logs

### Monitoring Details

What’s tracked: CPU, memory, QPS, logs
QPS Monitoring: Via Nginx logs or tools like collectd, Prometheus

### Task 2 Issues

SSL termination at Load Balancer may expose internal traffic
Single Primary DB still vulnerable
All-in-one servers (running all roles) limit scaling/fault isolation

## Task 3: Scale Up

### File: 3-scale_up

```rust
User -> DNS -> Load Balancer #1 <--> Load Balancer #2 (Clustered)
            ├─ Web Server #1         ├─ Web Server #2
            └─ App Server #1         └─ App Server #2
                         └──> MySQL (Dedicated Server)
```

### App Server vs Web Server

Component Role
Web Server Handles HTTP, static files, reverse proxy
App Server Executes backend logic, dynamic content

### New Additions

Clustered HAProxy LBs: High availability with failover (e.g. using Keepalived)
Split servers:
Web servers (Nginx)
Application servers (Flask/Node/PHP-FPM)
Database server (MySQL)

### Benefits

High Availability: Load balancer cluster prevents SPOF
Better Performance: Servers dedicated to roles
Easier Scaling: Horizontal scaling per layer

### Next Steps & Recommendations

Introduce read replicas for DB
Use CDNs for static assets
Implement auto-scaling and infra-as-code
Add caching layers like Redis/Varnish
Harden security with HTTPS internal traffic, stricter firewall rules, and zero-trust principles

## Repository Structure

```bash
alx-system_engineering-devops/
└── 0x09-web_infrastructure_design/
    ├── 0-simple_web_stack
    ├── 1-distributed_web_infrastructure
    ├── 2-secured_and_monitored_web_infrastructure
    └── 3-scale_up
    └── README.md
```
