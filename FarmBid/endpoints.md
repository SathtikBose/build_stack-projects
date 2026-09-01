# FarmBid – API Endpoints Specification

**Project Name:** FarmBid **Package Name:** `com.buildstack.farmbid` **API Type:** REST **Backend:** Node.js + Express.js **Database:** MongoDB **Authentication:** Firebase ID Token **Document Version:** 1.0 **Base Path:** `/api`

---

# 1. API Overview

FarmBid exposes REST APIs for:

- User profiles.
- Product management.
- Marketplace browsing.
- Bidding.
- Deal management.
- Chat.
- Messaging.

Authentication is performed using Firebase Authentication.

The Android application must send a Firebase ID token with every protected request.

---

# 2. Base URL

Development:

```text
http://localhost:5000/api
```

Production:

```text
https://your-farmbid-api.onrender.com/api
```

The production URL should be replaced with the final Render deployment URL.

---

# 3. Authentication

All protected endpoints require:

```http
Authorization: Bearer <FIREBASE_ID_TOKEN>
```

The backend validates this token using the Firebase Admin SDK.

---

# 4. Common Response Format

## 4.1 Success

```json
{
  "success": true,
  "message": "Request completed successfully",
  "data": {}
}
```

---

## 4.2 Error

```json
{
  "success": false,
  "message": "Validation failed",
  "errors": []
}
```

---

# 5. Health Endpoint

## Check API Status

```http
GET /health
```

Authentication: Not required.

### Response

```json
{
  "success": true,
  "message": "FarmBid API is running"
}
```

---

# 6. User Endpoints

---

## 6.1 Create User Profile

Creates the FarmBid application profile after Firebase registration.

```http
POST /api/users/profile
```

Authentication: Required.

### Request Body

```json
{
  "role": "FARMER",
  "fullName": "Rahul Kumar",
  "phoneNumber": "+919876543210",
  "language": "en",
  "location": {
    "address": "Jamshedpur, Jharkhand",
    "latitude": 22.8046,
    "longitude": 86.2029
  }
}
```

### Allowed Roles

```text
FARMER
BUYER
```

### Response

```json
{
  "success": true,
  "message": "Profile created successfully",
  "data": {
    "_id": "userObjectId",
    "role": "FARMER",
    "fullName": "Rahul Kumar"
  }
}
```

---

## 6.2 Get Current User

```http
GET /api/users/me
```

Authentication: Required.

### Response

```json
{
  "success": true,
  "message": "User retrieved successfully",
  "data": {}
}
```

---

## 6.3 Update Current User

```http
PUT /api/users/me
```

Authentication: Required.

### Request Body

```json
{
  "fullName": "Rahul Kumar",
  "phoneNumber": "+919876543210",
  "language": "hi",
  "location": {
    "address": "Jamshedpur, Jharkhand",
    "latitude": 22.8046,
    "longitude": 86.2029
  }
}
```

Users cannot change their role in the MVP.

---

## 6.4 Get User Public Profile

```http
GET /api/users/:id
```

Authentication: Required.

### Response

```json
{
  "success": true,
  "data": {
    "_id": "userObjectId",
    "fullName": "Rahul Kumar",
    "role": "FARMER",
    "location": {
      "address": "Jamshedpur, Jharkhand"
    }
  }
}
```

Sensitive data must not be exposed unnecessarily.

---

# 7. Product Endpoints

---

## 7.1 Create Product

```http
POST /api/products
```

Authentication: Required. Role: FARMER.

Content type:

```text
multipart/form-data
```

### Fields

```text
name
quantity
unit
basePrice
location
harvestDate
quality
images[]
```

### Validation

- Minimum 2 images.
- Maximum 4 images.
- Quantity greater than 0.
- Base price greater than 0.
- Harvest date cannot be in the future.

### Response

```json
{
  "success": true,
  "message": "Product created successfully",
  "data": {
    "_id": "productObjectId",
    "name": "Wheat",
    "quantity": 20,
    "unit": "KG",
    "status": "AVAILABLE"
  }
}
```

---

## 7.2 Get Marketplace Products

```http
GET /api/products
```

Authentication: Required.

### Query Parameters

```text
search
location
minPrice
maxPrice
sort
page
limit
```

### Example

```http
GET /api/products?search=wheat&location=jamshedpur&page=1&limit=20
```

### Sort Values

```text
newest
oldest
price_asc
price_desc
```

### Response

```json
{
  "success": true,
  "data": {
    "products": [],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 100,
      "totalPages": 5
    }
  }
}
```

---

## 7.3 Get Product Details

```http
GET /api/products/:id
```

Authentication: Required.

### Response

```json
{
  "success": true,
  "data": {
    "_id": "productObjectId",
    "name": "Wheat",
    "quantity": 20,
    "unit": "KG",
    "basePrice": 30,
    "quality": "Grade A",
    "images": [],
    "farmer": {}
  }
}
```

---

## 7.4 Update Product

```http
PUT /api/products/:id
```

Authentication: Required. Role: FARMER. Owner only.

### Request Body

```json
{
  "name": "Wheat",
  "quantity": 40,
  "unit": "KG",
  "basePrice": 32,
  "quality": "Grade A"
}
```

---

## 7.5 Delete Product

```http
DELETE /api/products/:id
```

Authentication: Required. Role: FARMER. Owner only.

The backend should reject deletion when an active deal exists.

---

## 7.6 Get My Products

```http
GET /api/products/my-products
```

Authentication: Required. Role: FARMER.

---

# 8. Bid Endpoints

---

## 8.1 Create Bid

```http
POST /api/products/:productId/bids
```

Authentication: Required. Role: BUYER.

### Request Body

```json
{
  "offeredPrice": 35,
  "requestedQuantity": 20
}
```

### Response

```json
{
  "success": true,
  "message": "Bid submitted successfully",
  "data": {
    "_id": "bidObjectId",
    "status": "PENDING"
  }
}
```

---

## 8.2 Get Product Bids

```http
GET /api/products/:productId/bids
```

Authentication: Required. Role: FARMER. Product owner only.

### Response

```json
{
  "success": true,
  "data": {
    "bids": []
  }
}
```

---

## 8.3 Get My Bids

```http
GET /api/bids/my-bids
```

Authentication: Required. Role: BUYER.

### Query Parameters

```text
status
page
limit
```

---

## 8.4 Update Bid

```http
PUT /api/bids/:id
```

Authentication: Required. Role: BUYER. Bid owner only.

### Request Body

```json
{
  "offeredPrice": 38,
  "requestedQuantity": 20
}
```

Only `PENDING` bids can be updated.

---

## 8.5 Cancel Bid

```http
POST /api/bids/:id/cancel
```

Authentication: Required. Role: BUYER. Bid owner only.

The bid status becomes:

```text
CANCELLED
```

---

## 8.6 Accept Bid

```http
POST /api/bids/:id/accept
```

Authentication: Required. Role: FARMER. Product owner only.

### Process

The backend must:

1. Validate the bid.
2. Verify the farmer owns the product.
3. Confirm that the bid is `PENDING`.
4. Confirm no bid was previously accepted.
5. Change bid status to `ACCEPTED`.
6. Create a deal.
7. Create a chat.
8. Reject other pending bids.
9. Change product status to `DEAL_CREATED`.

### Response

```json
{
  "success": true,
  "message": "Bid accepted and deal created successfully",
  "data": {
    "deal": {},
    "chat": {}
  }
}
```

---

## 8.7 Reject Bid

```http
POST /api/bids/:id/reject
```

Authentication: Required. Role: FARMER. Product owner only.

The bid status becomes:

```text
REJECTED
```

---

# 9. Deal Endpoints

---

## 9.1 Get My Deals

```http
GET /api/deals
```

Authentication: Required.

### Query Parameters

```text
status
page
limit
```

Users receive only deals in which they are participants.

---

## 9.2 Get Deal Details

```http
GET /api/deals/:id
```

Authentication: Required.

Only the farmer or buyer participating in the deal can access it.

---

## 9.3 Complete Deal

```http
POST /api/deals/:id/complete
```

Authentication: Required.

The user must be a deal participant.

The deal status becomes:

```text
COMPLETED
```

The product status becomes:

```text
SOLD
```

---

## 9.4 Cancel Deal

```http
POST /api/deals/:id/cancel
```

Authentication: Required.

The user must be a deal participant.

The deal status becomes:

```text
CANCELLED
```

The exact bid/product status behavior must be handled by backend business rules.

---

# 10. Chat Endpoints

---

## 10.1 Get My Chats

```http
GET /api/chats
```

Authentication: Required.

### Query Parameters

```text
page
limit
```

The response should return chats ordered by:

```text
lastMessageAt DESC
```

---

## 10.2 Get Chat Details

```http
GET /api/chats/:id
```

Authentication: Required.

Only chat participants can access the chat.

---

## 10.3 Get Chat Messages

```http
GET /api/chats/:id/messages
```

Authentication: Required.

### Query Parameters

```text
page
limit
```

Recommended response:

```json
{
  "success": true,
  "data": {
    "messages": [],
    "pagination": {}
  }
}
```

Messages should be sorted chronologically.

---

## 10.4 Send Message

```http
POST /api/chats/:id/messages
```

Authentication: Required.

### Request Body

```json
{
  "content": "Hello, when can we meet?",
  "messageType": "TEXT"
}
```

### Response

```json
{
  "success": true,
  "message": "Message sent successfully",
  "data": {}
}
```

---

## 10.5 Mark Messages as Read

```http
POST /api/chats/:id/read
```

Authentication: Required.

The backend marks messages sent by the other participant as read.

---

# 11. Chat Polling

The MVP does not use WebSockets.

Recommended polling endpoint:

```http
GET /api/chats/:id/messages?after=<timestamp>
```

The Android application can:

```text
Open Chat
   ↓
Fetch Messages
   ↓
Wait 5–10 Seconds
   ↓
Fetch New Messages
```

Polling must stop when the chat screen is closed.

---

# 12. Endpoint Authorization Summary

| Endpoint Group     |    Farmer |     Buyer |
| ------------------ | --------: | --------: |
| User profile       |       Yes |       Yes |
| Browse products    |       Yes |       Yes |
| Create product     |       Yes |        No |
| Edit own product   |       Yes |        No |
| Delete own product |       Yes |        No |
| Place bid          |        No |       Yes |
| Update own bid     |        No |       Yes |
| Cancel own bid     |        No |       Yes |
| View product bids  |       Yes |        No |
| Accept bid         |       Yes |        No |
| Reject bid         |       Yes |        No |
| View deals         |       Yes |       Yes |
| Complete deal      |       Yes |       Yes |
| Chat               | Deal only | Deal only |

---

# 13. Error Responses

## 400 – Bad Request

```json
{
  "success": false,
  "message": "Invalid request data",
  "errors": [
    {
      "field": "quantity",
      "message": "Quantity must be greater than zero"
    }
  ]
}
```

---

## 401 – Unauthorized

```json
{
  "success": false,
  "message": "Authentication required"
}
```

---

## 403 – Forbidden

```json
{
  "success": false,
  "message": "You do not have permission to perform this action"
}
```

---

## 404 – Not Found

```json
{
  "success": false,
  "message": "Product not found"
}
```

---

## 409 – Conflict

```json
{
  "success": false,
  "message": "Another bid has already been accepted for this product"
}
```

---

# 14. API Security Rules

Every protected endpoint must:

1. Verify the Firebase ID token.
2. Resolve the Firebase UID to the FarmBid user.
3. Check the user's role.
4. Verify resource ownership.
5. Validate the request.
6. Return a standardized response.

---

# 15. Required Middleware

```text
Request
   ↓
CORS
   ↓
Authentication Middleware
   ↓
Role Authorization
   ↓
Request Validation
   ↓
Controller
   ↓
Service
   ↓
MongoDB
   ↓
Response
```

Required middleware:

```text
auth.middleware.js
role.middleware.js
validate.middleware.js
error.middleware.js
not-found.middleware.js
```

---

# 16. API Testing Requirements

Every endpoint must be tested for:

- Valid request.
- Missing authentication.
- Invalid authentication.
- Invalid user role.
- Invalid request body.
- Invalid resource ID.
- Unauthorized resource access.
- Successful response.
- Database error handling.

Recommended test flow:

```text
IMPLEMENT
   ↓
TEST API
   ↓
FIX ERRORS
   ↓
TEST AGAIN
   ↓
INTEGRATE WITH ANDROID
   ↓
TEST FULL FLOW
```

---

# 17. Endpoint Summary

```text
HEALTH
GET    /health

USERS
POST   /api/users/profile
GET    /api/users/me
PUT    /api/users/me
GET    /api/users/:id

PRODUCTS
POST   /api/products
GET    /api/products
GET    /api/products/:id
PUT    /api/products/:id
DELETE /api/products/:id
GET    /api/products/my-products

BIDS
POST   /api/products/:productId/bids
GET    /api/products/:productId/bids
GET    /api/bids/my-bids
PUT    /api/bids/:id
POST   /api/bids/:id/cancel
POST   /api/bids/:id/accept
POST   /api/bids/:id/reject

DEALS
GET    /api/deals
GET    /api/deals/:id
POST   /api/deals/:id/complete
POST   /api/deals/:id/cancel

CHATS
GET    /api/chats
GET    /api/chats/:id
GET    /api/chats/:id/messages
POST   /api/chats/:id/messages
POST   /api/chats/:id/read
```

---

# 18. Final API Flow

```text
ANDROID APP
      │
      │ Firebase ID Token
      ▼
NODE.JS API
      │
      ▼
AUTHENTICATION
      │
      ▼
ROLE AUTHORIZATION
      │
      ▼
REQUEST VALIDATION
      │
      ▼
BUSINESS LOGIC
      │
      ├── MongoDB
      │
      └── Cloudinary
      │
      ▼
STANDARD API RESPONSE
```

These endpoints define the complete MVP backend interface for FarmBid.
