# High Level Design — Mini Instagram

## Infrastructure diagram

```mermaid
flowchart TB
    subgraph Users
        C[Client]
    end

    subgraph "User Platform"
        W[Web Browser]
    end

    C --> W

    subgraph WAF
        CF[Cloudflare]
    end
    W --> CF

    subgraph "Load Balancer"
        NG[Nginx]
    end
    CF --> NG

    subgraph "Application Servers"
        A1[Go App Node 1]
        A2[Go App Node 2]
        A3[Go App Node 3]
    end
    NG --> A1
    NG --> A2
    NG --> A3

    subgraph "Database"
        M[(PostgreSQL Master)]
        S[(PostgreSQL Slave)]
    end
    A1 --> M
    A2 --> M
    A3 --> M
    M --> S

    subgraph "Cache"
        R[(Redis)]
    end
    A1 --> R
    A2 --> R
    A3 --> R

    %% subgraph "Data Warehouse"
    %%     CH[(ClickHouse)]
    %% end
    %% A1 --> CH
    %% A2 --> CH
    %% A3 --> CH
```

## Component list

| Layer | Technology | Notes |
|---|---|---|
| User | Client | Web user |
| Platform | Web browser | Single responsive web UI |
| WAF | Cloudflare | DDoS, CDN, DNS |
| Load balancer | Nginx | Distributes traffic across 3 app nodes |
| Application server | Go (Golang) | 3 nodes, stateless |
| Database | PostgreSQL | Master + Slave (read replica) |
| Cache | Redis | Hot data / sessions |
| Data warehouse | ClickHouse | Cold / unused data archive |

## Traffic flow

1. Client opens the web app in a browser.
2. Cloudflare filters traffic and caches static assets.
3. Nginx load-balances requests across the 3 Go application nodes.
4. App nodes read/write to the **PostgreSQL master** for writes.
5. App nodes read from the **PostgreSQL slave** for read-heavy queries.
6. App nodes use **Redis** for caching and short-lived state.
7. App nodes move cold or unused data to **ClickHouse** for long-term storage.
