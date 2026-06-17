# Techee

## 🏗️ Architecture
 
Shopre follows a decoupled microservices architecture where each domain service is independently deployable and registers itself with Eureka for discovery. All external traffic is routed through a single API Gateway, which handles centralized routing, load balancing, and request filtering.
 
```text
                        ┌──────────────────────┐
                        │   React + TS Client  │
                        └──────────┬───────────┘
                                   │
                                   ▼
                        ┌───────────────────────┐
                        │  Spring Cloud API     │
                        │  Gateway (Routing)    │
                        └──────────┬────────────┘
                                   │
              ┌────────────────────┼────────────────────┐
              ▼                    ▼                     ▼
   ┌────────────────────┐ ┌───────────────────┐ ┌────────────────────────┐
   │  User Auth Service │ │ Product Service   │ │ Order & Cart Service   │
   │   (JWT + RBAC)     │ │ (Catalog)         │ │ (Orders, Cart, Txns)   │
   └──────────┬─────────┘ └──────────┬────────┘ └───────────┬────────────┘
              │                      │                      │
              └───────────────┬──────┴──────────────────────┘
                               ▼
                    ┌─────────────────────────┐
                    │ Eureka Service Registry │
                    └─────────────────────────┘
                               │
                               ▼
                    ┌────────────────────────┐
                    │     PostgreSQL DB(s)   │
                    └────────────────────────┘
```
 
**Core components:**
 
- **Service Discovery** — Eureka Service Registry handles dynamic service lookup, removing the need for hardcoded service URLs.
- **API Gateway** — Spring Cloud Gateway centralizes routing, request filtering, and acts as the single entry point for the frontend.
- **Domain Services** — Independently deployable services for Order & Cart, Product Catalog, and User Authentication (JWT + RBAC).
- **Orchestration** — `docker-compose.yml` spins up the full backend stack (services + database) for local development with a single command.


 
## 📂 Project Structure
 
```text
/ (Root)
├── backend/                          # Backend microservices root
│   ├── order-and-cart-management/    # Handles orders, shopping cart, and transactions
│   ├── product/                      # Manages product catalog and inventory
│   ├── service-discovery/            # Eureka Service Registry for service lookup
│   ├── shopre-api-gateway/           # Spring Cloud Gateway for centralized routing
│   ├── user-authentication/          # JWT-based Auth and RBAC service
│   └── docker-compose.yml            # Container orchestration for all services
│
└── frontend/                         # React + TypeScript frontend
    ├── src/
    ├── components/                   # Reusable ShadCN UI components
    └── ...
```
