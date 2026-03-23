# Payment Processing API

Complete API reference for the Payment Processing module.

## Authentication

All endpoints require authentication. Include JWT token in Authorization header:

```
Authorization: Bearer <token>
```

## Endpoints

### Payment Methods

#### GET /api/payment-methods

List available payment methods.

**Response (200):**
```json
{
  "methods": [
    {
      "id": "stripe",
      "name": "Credit Card (Stripe)",
      "enabled": true,
      "fees": {"percentage": 2.9, "fixed": 0.30}
    },
    {
      "id": "paypal",
      "name": "PayPal",
      "enabled": true,
      "fees": {"percentage": 3.49, "fixed": 0.49}
    }
  ]
}
```

### Payment Endpoints

#### POST /api/payments

Create a payment intent.

**Request Body:**
```json
{
  "amount": 6598,
  "currency": "usd",
  "method": "stripe",
  "orderId": "order_id",
  "metadata": {"cartId": "cart_id"}
}
```

**Response (201):**
```json
{
  "id": "payment_id",
  "status": "requires_payment_method",
  "amount": 6598,
  "currency": "usd",
  "clientSecret": "pi_123_secret_abc",
  "method": "stripe"
}
```

#### GET /api/payments/:id

Get payment details.

**Response (200):**
```json
{
  "id": "payment_id",
  "status": "succeeded",
  "amount": 6598,
  "currency": "usd",
  "method": "stripe",
  "createdAt": "2024-01-15T10:30:00Z",
  "orderId": "order_id"
}
```

#### POST /api/payments/:id/capture

Capture a payment (for authorized payments).

**Response (200):**
```json
{
  "id": "payment_id",
  "status": "succeeded",
  "amount": 6598,
  "capturedAt": "2024-01-15T10:35:00Z"
}
```

### Refund Endpoints

#### POST /api/refunds

Create a refund.

**Request Body:**
```json
{
  "paymentId": "payment_id",
  "amount": 6598,
  "reason": "customer_request"
}
```

**Response (201):**
```json
{
  "id": "refund_id",
  "status": "succeeded",
  "amount": 6598,
  "currency": "usd",
  "paymentId": "payment_id",
  "createdAt": "2024-01-15T11:00:00Z"
}
```

### Transaction Endpoints

#### GET /api/transactions

List transactions.

**Query Parameters:**
- `page` - Page number (default: 1)
- `limit` - Items per page (default: 20)
- `status` - Filter by status
- `method` - Filter by payment method

**Response (200):**
```json
{
  "transactions": [
    {
      "id": "txn_id",
      "type": "payment",
      "status": "succeeded",
      "amount": 6598,
      "currency": "usd",
      "method": "stripe",
      "createdAt": "2024-01-15T10:30:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 150
  }
}
```

## Webhooks

### Webhook Endpoints

Webhook endpoints receive payment status updates from payment providers.

#### POST /api/webhooks/stripe

Stripe webhook endpoint.

**Headers:**
- `Stripe-Signature` - HMAC signature for verification

**Event Types:**
- `payment_intent.succeeded` - Payment completed
- `payment_intent.payment_failed` - Payment failed
- `charge.refunded` - Refund processed

#### POST /api/webhooks/paypal

PayPal webhook endpoint.

**Headers:**
- `PayPal-Transmission-Id` - Transmission ID for verification
- `PayPal-Cert-Id` - Certificate ID for verification

**Event Types:**
- `PAYMENT.CAPTURE.COMPLETED` - Payment completed
- `PAYMENT.CAPTURE.DENIED` - Payment denied

## Payment States

Payments transition through these states:

```
requires_payment_method → requires_confirmation → requires_action → processing → succeeded
                                                                ↓
                                                             cancelled/failed
```

**State descriptions:**
- `requires_payment_method` - Payment intent created, awaiting payment method
- `requires_confirmation` - Payment method provided, awaiting confirmation
- `requires_action` - Additional authentication required (3D Secure)
- `processing` - Payment being processed
- `succeeded` - Payment completed successfully
- `cancelled` - Payment cancelled by user
- `failed` - Payment failed (insufficient funds, declined, etc.)

## Error Responses

### 400 Bad Request
```json
{
  "error": "Invalid amount",
  "details": ["Amount must be positive"]
}
```

### 402 Payment Required
```json
{
  "error": "Payment failed",
  "message": "Card declined"
}
```

### 409 Conflict
```json
{
  "error": "Payment already captured"
}
```

## See Also

- [Module README](../README.md)
