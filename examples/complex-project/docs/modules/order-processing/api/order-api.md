# Order Processing API

Complete API reference for the Order Processing module.

## Authentication

All endpoints require authentication. Include JWT token in Authorization header:

```
Authorization: Bearer <token>
```

## Endpoints

### Cart Endpoints

#### GET /api/cart

Get user's shopping cart.

**Response (200):**
```json
{
  "id": "cart_id",
  "userId": "user_id",
  "items": [
    {
      "id": "item_id",
      "productId": "prod_id",
      "name": "Product Name",
      "price": 29.99,
      "quantity": 2,
      "subtotal": 59.98
    }
  ],
  "totals": {
    "subtotal": 59.98,
    "tax": 6.00,
    "total": 65.98
  }
}
```

#### POST /api/cart/items

Add item to cart.

**Request Body:**
```json
{
  "productId": "prod_id",
  "quantity": 2
}
```

**Response (201):**
```json
{
  "id": "new_item_id",
  "productId": "prod_id",
  "quantity": 2,
  "subtotal": 59.98
}
```

#### PUT /api/cart/items/:id

Update cart item quantity.

**Request Body:**
```json
{
  "quantity": 3
}
```

**Response (200):**
```json
{
  "id": "item_id",
  "quantity": 3,
  "subtotal": 89.97
}
```

#### DELETE /api/cart/items/:id

Remove item from cart.

**Response (204):** No content

### Checkout Endpoints

#### POST /api/checkout

Start checkout process.

**Request Body:**
```json
{
  "shippingAddress": {
    "name": "John Doe",
    "street": "123 Main St",
    "city": "New York",
    "zip": "10001",
    "country": "US"
  },
  "paymentMethod": "stripe"
}
```

**Response (200):**
```json
{
  "orderId": "order_id",
  "status": "pending_payment",
  "paymentIntent": {
    "id": "pi_123",
    "amount": 6598,
    "currency": "usd",
    "clientSecret": "pi_123_secret_abc"
  }
}
```

### Order Endpoints

#### GET /api/orders/:id

Get order details.

**Response (200):**
```json
{
  "id": "order_id",
  "status": "confirmed",
  "createdAt": "2024-01-15T10:30:00Z",
  "items": [
    {
      "productId": "prod_id",
      "name": "Product Name",
      "price": 29.99,
      "quantity": 2
    }
  ],
  "shippingAddress": {
    "name": "John Doe",
    "street": "123 Main St",
    "city": "New York",
    "zip": "10001",
    "country": "US"
  },
  "totals": {
    "subtotal": 59.98,
    "tax": 6.00,
    "total": 65.98
  },
  "payment": {
    "status": "paid",
    "method": "stripe"
  }
}
```

#### GET /api/orders

List user's orders.

**Query Parameters:**
- `page` - Page number (default: 1)
- `limit` - Items per page (default: 10)
- `status` - Filter by status

**Response (200):**
```json
{
  "orders": [
    {
      "id": "order_id",
      "status": "confirmed",
      "createdAt": "2024-01-15T10:30:00Z",
      "total": 65.98
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 25
  }
}
```

#### PUT /api/orders/:id/cancel

Cancel an order.

**Response (200):**
```json
{
  "id": "order_id",
  "status": "cancelled",
  "cancelledAt": "2024-01-15T11:00:00Z"
}
```

## Order States

Orders transition through these states:

```
pending → pending_payment → confirmed → processing → shipped → delivered
                                                    ↓
                                                 cancelled
```

**State descriptions:**
- `pending` - Cart saved, checkout not started
- `pending_payment` - Checkout initiated, awaiting payment
- `confirmed` - Payment received, processing started
- `processing` - Being prepared for shipment
- `shipped` - Shipped, tracking available
- `delivered` - Delivered to customer
- `cancelled` - Order cancelled (refund initiated if paid)

## Error Responses

### 400 Bad Request
```json
{
  "error": "Validation Error",
  "details": ["quantity must be positive"]
}
```

### 409 Conflict
```json
{
  "error": "Insufficient inventory"
}
```

## See Also

- [Module README](../README.md)
