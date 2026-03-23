# Architecture Diagrams

This directory contains architecture diagrams for the E-Commerce Platform.

## Files

### architecture.png

System architecture diagram showing:

### architecture.drawio

Source file for the architecture diagram (commit alongside .png)

**Components:**
- API Gateway (Kong) - Routes requests to backend services
- User Management Service - Authentication, profiles, preferences
- Product Catalog Service - Products, categories, search
- Order Processing Service - Cart, checkout, order lifecycle
- Payment Processing Service - Payment gateway integration

**Infrastructure:**
- PostgreSQL - Primary database for all services
- Redis - Caching layer
- RabbitMQ - Message queue for async communication
- Elasticsearch - Product search indexing

**Data Flow:**
1. Client → API Gateway
2. API Gateway → Services (based on route)
3. Services ↔ PostgreSQL (persistent storage)
4. Services ↔ Redis (caching)
5. Services → RabbitMQ (events)
6. Product Catalog → Elasticsearch (search)

## Creating the Diagram

Use [draw.io](https://diagrams.net) to create the diagram:

1. Open https://diagrams.net
2. Create new blank diagram
3. Add rectangles for services/components
4. Add cylinders for databases
5. Add arrows showing relationships
6. Save the source file as `architecture.drawio`
7. Export as PNG to `architecture.png`

**Recommended layout:** Horizontal flow with API Gateway on left, services in middle, infrastructure on right.

## See Also

- [Diagrams Guide](../../../../writing-guide/06-diagrams.md) - Comprehensive diagram standards

---

**Last Updated:** 2026-03-23
