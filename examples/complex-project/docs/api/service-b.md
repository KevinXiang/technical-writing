# Product Service API Documentation

> Product catalog and search service API

## Overview

The Product Service manages product catalog, categories, pricing, and provides search functionality.

## Base URL

```
[Production] https://api.example.com/product/v1
[Staging] https://api-staging.example.com/product/v1
[Development] http://localhost:3002/v1
```

## Authentication

Most endpoints require authentication. Public endpoints are marked as such.

```http
Authorization: Bearer <access-token>
Content-Type: application/json
```

## Endpoints

### List Products

Retrieve paginated list of products with filtering and sorting.

```http
GET /products
```

**Query Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `page` | integer | No | Page number (default: 1) |
| `perPage` | integer | No | Items per page (default: 20, max: 100) |
| `category` | string | No | Filter by category slug |
| `minPrice` | number | No | Minimum price |
| `maxPrice` | number | No | Maximum price |
| `search` | string | No | Search query |
| `sort` | string | No | Sort field (name, price, createdAt) |
| `order` | string | No | Sort order (asc, desc) |

**Response:** `200 OK`

```json
{
  "data": [
    {
      "id": "prod-123",
      "name": "Product Name",
      "description": "Product description",
      "price": 29.99,
      "compareAtPrice": 39.99,
      "sku": "PROD-001",
      "category": {
        "id": "cat-123",
        "name": "Electronics",
        "slug": "electronics"
      },
      "images": [
        {
          "url": "https://cdn.example.com/product-1.jpg",
          "alt": "Product image"
        }
      ],
      "inventory": 100,
      "inStock": true,
      "createdAt": "2024-01-01T00:00:00Z",
      "updatedAt": "2024-01-02T00:00:00Z"
    }
  ],
  "meta": {
    "page": 1,
    "perPage": 20,
    "totalPages": 5,
    "totalItems": 100
  }
}
```

### Get Product

Retrieve single product by ID.

```http
GET /products/:id
```

**Path Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes | Product ID or slug |

**Response:** `200 OK`

```json
{
  "data": {
    "id": "prod-123",
    "name": "Product Name",
    "description": "Product description",
    "details": "<p>Detailed HTML description</p>",
    "price": 29.99,
    "compareAtPrice": 39.99,
    "sku": "PROD-001",
    "category": {
      "id": "cat-123",
      "name": "Electronics",
      "slug": "electronics"
    },
    "images": [
      {
        "url": "https://cdn.example.com/product-1.jpg",
        "alt": "Product image",
        "position": 1
      },
      {
        "url": "https://cdn.example.com/product-2.jpg",
        "alt": "Product image 2",
        "position": 2
      }
    ],
    "variants": [
      {
        "id": "var-123",
        "name": "Small / Red",
        "price": 29.99,
        "inventory": 10,
        "attributes": {
          "size": "S",
          "color": "Red"
        }
      }
    ],
    "attributes": {
      "material": "Cotton",
      "brand": "Brand Name",
      "weight": "1.5 lbs"
    },
    "inventory": 100,
    "inStock": true,
    "seo": {
      "title": "Product Name - Store Name",
      "description": "Product description for SEO",
      "keywords": ["product", "keyword"]
    },
    "createdAt": "2024-01-01T00:00:00Z",
    "updatedAt": "2024-01-02T00:00:00Z"
  }
}
```

### Search Products

Full-text search across products.

```http
GET /products/search
```

**Query Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `q` | string | Yes | Search query |
| `page` | integer | No | Page number (default: 1) |
| `perPage` | integer | No | Items per page (default: 20) |
| `category` | string | No | Filter by category |
| `minPrice` | number | No | Minimum price |
| `maxPrice` | number | No | Maximum price |

**Response:** `200 OK` (same format as list products)

### Create Product

Create a new product (requires admin role).

```http
POST /products
```

**Request Body:**

```json
{
  "name": "Product Name",
  "description": "Product description",
  "price": 29.99,
  "compareAtPrice": 39.99,
  "sku": "PROD-001",
  "categoryId": "cat-123",
  "images": [
    {
      "url": "https://cdn.example.com/product-1.jpg",
      "alt": "Product image"
    }
  ],
  "inventory": 100,
  "attributes": {
    "material": "Cotton",
    "brand": "Brand Name"
  }
}
```

**Response:** `201 Created`

Returns the created product.

### Update Product

Update existing product (requires admin role).

```http
PUT /products/:id
```

**Request Body:** Same as create product

**Response:** `200 OK`

Returns the updated product.

### Delete Product

Delete product (requires admin role).

```http
DELETE /products/:id
```

**Response:** `204 No Content`

### List Categories

Get all product categories.

```http
GET /categories
```

**Response:** `200 OK`

```json
{
  "data": [
    {
      "id": "cat-123",
      "name": "Electronics",
      "slug": "electronics",
      "description": "Electronic products",
      "parentId": null,
      "image": "https://cdn.example.com/category.jpg",
      "productCount": 150,
      "children": [
        {
          "id": "cat-124",
          "name": "Computers",
          "slug": "computers",
          "productCount": 50
        }
      ]
    }
  ]
}
```

### Get Category

Get single category with products.

```http
GET /categories/:slug
```

**Response:** `200 OK`

```json
{
  "data": {
    "id": "cat-123",
    "name": "Electronics",
    "slug": "electronics",
    "description": "Electronic products",
    "image": "https://cdn.example.com/category.jpg",
    "products": {
      "data": [/* products */],
      "meta": {/* pagination */}
    }
  }
}
```

## Data Models

### Product

```typescript
interface Product {
  id: string;
  name: string;
  description: string;
  details?: string;
  price: number;
  compareAtPrice?: number;
  sku: string;
  category: Category;
  images: ProductImage[];
  variants?: ProductVariant[];
  attributes?: Record<string, any>;
  inventory: number;
  inStock: boolean;
  seo?: SEO;
  createdAt: string;
  updatedAt: string;
}
```

### ProductImage

```typescript
interface ProductImage {
  url: string;
  alt: string;
  position?: number;
}
```

### ProductVariant

```typescript
interface ProductVariant {
  id: string;
  name: string;
  price: number;
  inventory: number;
  attributes: Record<string, string>;
}
```

### Category

```typescript
interface Category {
  id: string;
  name: string;
  slug: string;
  description?: string;
  parentId?: string;
  image?: string;
  productCount?: number;
  children?: Category[];
}
```

## Error Codes

| Code | Description |
|------|-------------|
| `PRODUCT_NOT_FOUND` | Product not found |
| `CATEGORY_NOT_FOUND` | Category not found |
| `INVALID_PRICE` | Invalid price value |
| `INVALID_SKU` | SKU already exists or invalid |
| `OUT_OF_STOCK` | Product out of stock |

## Examples

### Search with Filters

```javascript
const response = await fetch(
  'https://api.example.com/product/v1/products/search?q=laptop&category=electronics&minPrice=500&maxPrice=1500&sort=price&order=asc&page=1&perPage=20',
  {
    headers: {
      'Authorization': `Bearer ${token}`
    }
  }
);

const { data, meta } = await response.json();
console.log(`Found ${meta.totalItems} products`);
```

---

**API Version:** 1.0.0
**Last Updated:** 2024-03-23
