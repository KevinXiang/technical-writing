# Product Catalog

> Product search, categorization, and inventory display

## Overview

The Product Catalog module manages all product-related functionality including product listings, categorization, search, and pricing. Built with FastAPI for high performance and Elasticsearch for powerful search capabilities.

This module is responsible for product CRUD operations, category hierarchy management, product search with filtering, and real-time inventory display.

## Architecture

![Product Flow](images/product-flow.png)

### Key Components

- **Product Service** - Handles product CRUD and inventory queries
- **Category Service** - Manages category hierarchy and relationships
- **Search Service** - Integrates with Elasticsearch for product search
- **Pricing Service** - Manages product pricing and discounts

## Tech Stack

- **Language:** Python 3.11
- **Framework:** FastAPI
- **Database:** PostgreSQL 15
- **Search:** Elasticsearch 8
- **ORM:** SQLAlchemy 2.0

## API Documentation

### Core Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/products | List products with filtering |
| GET | /api/products/:id | Get product details |
| POST | /api/products | Create product (admin) |
| PUT | /api/products/:id | Update product (admin) |
| GET | /api/categories | List categories |
| POST | /api/categories | Create category (admin) |
| GET | /api/search | Search products |

For complete API reference, see [API Documentation](api/product-api.md)

## Configuration

### Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| DB_HOST | Yes | localhost | Database host |
| DB_PORT | No | 5432 | Database port |
| DB_NAME | Yes | products | Database name |
| ELASTICSEARCH_URL | Yes | http://localhost:9200 | Elasticsearch URL |
| SEARCH_INDEX | No | products | Search index name |

## Development

### Prerequisites

- Python 3.11
- PostgreSQL 15
- Elasticsearch 8
- Running database infrastructure

### Setup

```bash
# Navigate to module directory
cd modules/product-catalog

# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Set up environment
cp .env.example .env

# Run database migrations
alembic upgrade head

# Seed database
python seed_data.py
```

### Running Locally

```bash
# Activate virtual environment
source venv/bin/activate

# Start development server
uvicorn main:app --reload --port 3002

# Server runs on http://localhost:3002
# API docs available at http://localhost:3002/docs
```

### Common Tasks

```bash
# Run tests
pytest

# Run with coverage
pytest --cov=src --cov-report=html

# Format code
black src/
```

## Testing

```bash
# Unit tests
pytest tests/unit/

# Integration tests
pytest tests/integration/

# E2E tests
pytest tests/e2e/

# All tests
pytest
```

## Deployment

For deployment instructions, see [Deployment Guide](../../../docs/deployment.md)

## Troubleshooting

### Common Issues

**Issue:** Search returns no results
**Solution:** Rebuild Elasticsearch index with `python rebuild_index.py`

**Issue:** Product not showing in catalog
**Solution:** Ensure product is active and inventory > 0

**Issue:** Category hierarchy not loading
**Solution:** Check parent_category_id references exist

## FAQ

### How do I add product attributes?

Add custom attributes in product_data JSON field. Query with `?filter=attributes.key:value`.

### How is search ranking determined?

Elasticsearch uses BM25 scoring with boosts for title, category, and description fields.

### Can I have multiple category levels?

Yes, categories support unlimited nesting using parent_category_id.

## See Also

- [Main README](../../../README.md)
- [API Documentation](api/product-api.md)
- [Deployment Guide](../../../docs/deployment.md)
