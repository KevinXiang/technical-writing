# Product Catalog API

Complete API reference for the Product Catalog module.

## Authentication

Admin endpoints require authentication. Include JWT token in Authorization header:

```
Authorization: Bearer <token>
```

## Endpoints

### Product Endpoints

#### GET /api/products

List products with optional filtering.

**Query Parameters:**
- `page` - Page number (default: 1)
- `limit` - Items per page (default: 20)
- `category` - Filter by category ID
- `min_price` - Minimum price filter
- `max_price` - Maximum price filter
- `sort` - Sort order: price_asc, price_desc, name, newest

**Response (200):**
```json
{
  "products": [
    {
      "id": "prod_id",
      "name": "Product Name",
      "description": "Description",
      "price": 29.99,
      "category": {
        "id": "cat_id",
        "name": "Category Name"
      },
      "inventory": 100,
      "images": ["https://..."],
      "attributes": {"color": "red", "size": "M"}
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 150
  }
}
```

#### GET /api/products/:id

Get product details by ID.

**Response (200):**
```json
{
  "id": "prod_id",
  "name": "Product Name",
  "description": "Full description",
  "price": 29.99,
  "category": {
    "id": "cat_id",
    "name": "Category Name",
    "path": "Parent > Child"
  },
  "inventory": 100,
  "images": ["https://..."],
  "attributes": {"color": "red"},
  "variants": [
    {"id": "var_id", "name": "Size M", "price": 29.99, "inventory": 50}
  ],
  "reviews": {
    "average": 4.5,
    "count": 120
  }
}
```

#### POST /api/products

Create new product (admin only).

**Request Body:**
```json
{
  "name": "Product Name",
  "description": "Description",
  "price": 29.99,
  "category_id": "cat_id",
  "inventory": 100,
  "images": ["https://..."],
  "attributes": {"color": "red", "size": "M"}
}
```

**Response (201):**
```json
{
  "id": "new_prod_id",
  "name": "Product Name",
  "price": 29.99
}
```

#### PUT /api/products/:id

Update product (admin only).

**Request Body:**
```json
{
  "price": 24.99,
  "inventory": 75
}
```

**Response (200):**
```json
{
  "id": "prod_id",
  "name": "Product Name",
  "price": 24.99,
  "inventory": 75
}
```

### Category Endpoints

#### GET /api/categories

List all categories.

**Query Parameters:**
- `parent_id` - Filter by parent category (null for root categories)

**Response (200):**
```json
{
  "categories": [
    {
      "id": "cat_id",
      "name": "Electronics",
      "slug": "electronics",
      "parent_id": null,
      "children": [
        {"id": "child_id", "name": "Smartphones", "slug": "smartphones"}
      ]
    }
  ]
}
```

#### POST /api/categories

Create new category (admin only).

**Request Body:**
```json
{
  "name": "Category Name",
  "slug": "category-name",
  "parent_id": "parent_cat_id"
}
```

**Response (201):**
```json
{
  "id": "new_cat_id",
  "name": "Category Name",
  "slug": "category-name"
}
```

### Search Endpoint

#### GET /api/search

Search products using Elasticsearch.

**Query Parameters:**
- `q` - Search query
- `page` - Page number (default: 1)
- `filters` - JSON-encoded filters: `{"category": "cat_id", "price": {"min": 10, "max": 50}}`

**Response (200):**
```json
{
  "results": [
    {
      "product": {...},
      "score": 2.5,
      "highlight": {
        "name": "Wireless <em>Headphones</em>",
        "description": "Premium <em>headphones</em> with..."
      }
    }
  ],
  "total": 45,
  "pagination": {"page": 1, "limit": 20}
}
```

## Error Responses

### 400 Bad Request
```json
{
  "error": "Validation Error",
  "details": ["price must be positive"]
}
```

### 404 Not Found
```json
{
  "error": "Product not found"
}
```

### 422 Unprocessable Entity
```json
{
  "error": "Invalid category_id"
}
```

## See Also

- [Module README](../README.md)
