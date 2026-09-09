# BookWala — API Specification

**Project Name:** BookWala  
**Android Package:** `com.buildstack.bookwala`  
**Backend:** Node.js + Express.js  
**Database:** MongoDB  
**Authentication:** Firebase Authentication  
**AI:** NVIDIA API / NVIDIA NIM-compatible API  
**Real-Time:** WebSocket  
**Image Storage:** Cloudinary  
**Version:** 1.0

---

# 1. Purpose

This document defines the REST API contract between the BookWala Android application and the BookWala backend.

It covers:

- Authentication
- User profiles
- Marketplace
- Book listings
- Search
- Filtering
- Chat
- Messages
- AI book discovery
- Cloudinary image handling
- Error responses
- Pagination
- Authorization
- WebSocket integration

The Android application must communicate with application data through these APIs.

---

# 2. Base URL

Development:

```text
http://localhost:<PORT>
```

Production:

```text
https://<bookwala-backend-domain>
```

The production backend will be hosted on Render.

API prefix:

```text
/api
```

Therefore:

```text
https://<bookwala-backend-domain>/api
```

---

# 3. API Architecture

```mermaid
flowchart LR

    A[Android App] --> B[REST API]

    B --> C[Express Router]
    C --> D[Authentication Middleware]
    D --> E[Controller]
    E --> F[Service]
    F --> G[(MongoDB)]

    F --> H[Cloudinary]
    F --> I[NVIDIA API]
```

---

# 4. Authentication Architecture

Firebase Authentication is used for identity.

The Android client obtains a Firebase ID token.

The token is sent to the BookWala backend.

```text
Android
 ↓
Firebase Authentication
 ↓
Firebase ID Token
 ↓
Authorization Header
 ↓
BookWala Backend
 ↓
Verify Firebase Token
 ↓
Authenticated Request
```

---

# 5. Authorization Header

Protected requests must include:

```http
Authorization: Bearer <FIREBASE_ID_TOKEN>
```

Example:

```http
Authorization: Bearer eyJhbGciOiJSUzI1NiIs...
```

The token must never be manually trusted.

The backend must verify it using Firebase Admin SDK.

---

# 6. Authentication States

Backend should distinguish:

```text
No token
Invalid token
Expired token
Valid token
```

Typical responses:

```text
401 Unauthorized
```

for missing/invalid authentication.

```text
403 Forbidden
```

for authenticated users without permission to perform an operation.

---

# 7. Response Format

Successful responses should follow a consistent structure.

Example:

```json
{
  "success": true,
  "data": {},
  "message": "Operation successful"
}
```

For lists:

```json
{
  "success": true,
  "data": [],
  "pagination": {},
  "message": "Books retrieved successfully"
}
```

---

# 8. Error Response Format

All API errors should use:

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message"
  }
}
```

Example:

```json
{
  "success": false,
  "error": {
    "code": "BOOK_NOT_FOUND",
    "message": "The requested book could not be found."
  }
}
```

Internal implementation details must not be exposed.

---

# 9. HTTP Status Codes

| Status | Meaning |
|---:|---|
| `200` | Successful request |
| `201` | Resource created |
| `204` | Successful operation with no response body |
| `400` | Invalid request |
| `401` | Authentication required/invalid |
| `403` | Permission denied |
| `404` | Resource not found |
| `409` | Conflict |
| `422` | Validation failure |
| `429` | Rate limit exceeded |
| `500` | Internal server error |
| `503` | Service unavailable |

---

# 10. API Modules

```text
/api/auth
/api/users
/api/books
/api/search
/api/chats
/api/ai
```

WebSocket uses a separate real-time endpoint.

---

# 11. Health Check

## GET `/api/health`

Used to determine whether the backend is running.

### Response

```json
{
  "success": true,
  "data": {
    "status": "ok"
  },
  "message": "BookWala API is running"
}
```

---

# 12. User APIs

---

## 12.1 Get Current User

### GET

```text
/api/users/me
```

Authentication:

```text
Required
```

### Response

```json
{
  "success": true,
  "data": {
    "id": "USER_ID",
    "firebaseUid": "FIREBASE_UID",
    "email": "user@example.com",
    "displayName": "John",
    "photoUrl": null,
    "phone": null,
    "bio": null,
    "isOnline": true,
    "lastSeenAt": null,
    "createdAt": "2026-09-07T10:00:00.000Z"
  },
  "message": "User retrieved successfully"
}
```

---

# 13. Create / Synchronize User

### POST

```text
/api/users/me
```

Authentication:

```text
Required
```

Purpose:

Create the BookWala user profile after successful Firebase authentication if it does not already exist.

### Request

```json
{
  "displayName": "John",
  "photoUrl": "https://example.com/photo.jpg"
}
```

The backend obtains the Firebase UID and email from the verified authentication token.

The client must not be allowed to choose another user's Firebase UID.

---

# 14. Update Current User

### PUT

```text
/api/users/me
```

Authentication:

```text
Required
```

### Request

```json
{
  "displayName": "John Doe",
  "phone": "+91XXXXXXXXXX",
  "bio": "Book lover"
}
```

### Response

```json
{
  "success": true,
  "data": {
    "id": "USER_ID",
    "displayName": "John Doe",
    "phone": "+91XXXXXXXXXX",
    "bio": "Book lover"
  },
  "message": "Profile updated successfully"
}
```

---

# 15. Get Public User

### GET

```text
/api/users/:userId
```

Authentication:

```text
Optional
```

Only public profile information should be returned.

Sensitive account information must not be exposed.

---

# 16. Book APIs

---

# 17. Get Marketplace Books

### GET

```text
/api/books
```

Authentication:

```text
Optional
```

Only active listings should be returned by default.

---

## 17.1 Query Parameters

```text
page
limit
search
category
author
edition
minPrice
maxPrice
condition
sort
```

Example:

```text
/api/books?page=1&limit=20&search=java&maxPrice=500
```

---

# 18. Pagination

Default:

```text
page = 1
limit = 20
```

Maximum recommended:

```text
limit = 100
```

Example response:

```json
{
  "success": true,
  "data": [
    {
      "id": "BOOK_ID",
      "title": "Java Programming",
      "author": "Author Name",
      "edition": "5th Edition",
      "price": 450,
      "description": "Good condition.",
      "imageUrl": "https://...",
      "sellerId": "USER_ID",
      "status": "active"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "totalItems": 45,
    "totalPages": 3,
    "hasNextPage": true
  },
  "message": "Books retrieved successfully"
}
```

---

# 19. Get Single Book

### GET

```text
/api/books/:bookId
```

Authentication:

```text
Optional
```

### Response

```json
{
  "success": true,
  "data": {
    "id": "BOOK_ID",
    "title": "Java Programming",
    "author": "Author Name",
    "edition": "5th Edition",
    "price": 450,
    "description": "Good condition.",
    "imageUrl": "https://...",
    "category": "Programming",
    "condition": "good",
    "status": "active",
    "seller": {
      "id": "USER_ID",
      "displayName": "John",
      "photoUrl": null,
      "isOnline": true,
      "lastSeenAt": null
    },
    "createdAt": "2026-09-07T10:00:00.000Z",
    "updatedAt": "2026-09-07T10:00:00.000Z"
  },
  "message": "Book retrieved successfully"
}
```

---

# 20. Create Book Listing

### POST

```text
/api/books
```

Authentication:

```text
Required
```

The authenticated user becomes the seller.

---

## 20.1 Request

The preferred MVP flow is:

```text
Image Upload
      ↓
Cloudinary
      ↓
imageUrl + imagePublicId
      ↓
Create Book API
```

### Request Body

```json
{
  "title": "Java Programming",
  "author": "Author Name",
  "edition": "5th Edition",
  "price": 450,
  "description": "Used but in good condition.",
  "imageUrl": "https://res.cloudinary.com/...",
  "imagePublicId": "bookwala/books/abc123",
  "category": "Programming",
  "condition": "good"
}
```

---

# 21. Create Book Response

```json
{
  "success": true,
  "data": {
    "id": "BOOK_ID",
    "title": "Java Programming",
    "author": "Author Name",
    "edition": "5th Edition",
    "price": 450,
    "description": "Used but in good condition.",
    "imageUrl": "https://res.cloudinary.com/...",
    "imagePublicId": "bookwala/books/abc123",
    "category": "Programming",
    "condition": "good",
    "sellerId": "USER_ID",
    "status": "active",
    "createdAt": "2026-09-07T10:00:00.000Z",
    "updatedAt": "2026-09-07T10:00:00.000Z"
  },
  "message": "Book listed successfully"
}
```

---

# 22. Update Book Listing

### PUT

```text
/api/books/:bookId
```

Authentication:

```text
Required
```

Authorization:

```text
Current user must own the listing.
```

### Request

```json
{
  "title": "Java Programming",
  "author": "Author Name",
  "edition": "6th Edition",
  "price": 500,
  "description": "Updated description.",
  "category": "Programming",
  "condition": "like_new"
}
```

---

# 23. Delete Book Listing

### DELETE

```text
/api/books/:bookId
```

Authentication:

```text
Required
```

Authorization:

```text
Current user must own the listing.
```

Preferred behavior:

```text
active → removed
```

rather than immediately deleting the document.

### Response

```json
{
  "success": true,
  "data": null,
  "message": "Book listing removed successfully"
}
```

---

# 24. Mark Book as Sold

### PATCH

```text
/api/books/:bookId/status
```

Authentication:

```text
Required
```

Authorization:

```text
Current user must own the listing.
```

### Request

```json
{
  "status": "sold"
}
```

Allowed:

```text
active
sold
removed
```

---

# 25. My Listings

### GET

```text
/api/users/me/books
```

Authentication:

```text
Required
```

Query:

```text
page
limit
status
```

Example:

```text
/api/users/me/books?page=1&limit=20&status=active
```

---

# 26. Search API

### GET

```text
/api/search/books
```

Authentication:

```text
Optional
```

Query:

```text
q
page
limit
```

Example:

```text
/api/search/books?q=python&page=1&limit=20
```

The search service should search actual book fields.

---

# 27. Advanced Book Search

### GET

```text
/api/search/books
```

Example:

```text
/api/search/books?q=programming&category=programming&maxPrice=500&condition=good
```

The backend must validate every query parameter.

---

# 28. Search Result Requirements

Search results must return actual marketplace listings.

Search must not:

- Generate fake books.
- Generate fake prices.
- Generate fake sellers.
- Return inactive listings by default.

---

# 29. Cloudinary Image Upload

The exact upload architecture may use either:

```text
Android → Cloudinary directly
```

with a secure upload mechanism,

or:

```text
Android → Backend → Cloudinary
```

For the MVP, the preferred architecture is to use a secure Cloudinary upload strategy and then send the resulting asset reference to the backend.

The Cloudinary secret must never be included in the Android application.

---

# 30. Chat APIs

---

# 31. Get Chats

### GET

```text
/api/chats
```

Authentication:

```text
Required
```

Returns conversations in which the authenticated user participates.

### Response

```json
{
  "success": true,
  "data": [
    {
      "id": "CHAT_ID",
      "buyerId": "BUYER_ID",
      "sellerId": "SELLER_ID",
      "book": {
        "id": "BOOK_ID",
        "title": "Java Programming",
        "imageUrl": "https://..."
      },
      "otherUser": {
        "id": "USER_ID",
        "displayName": "John",
        "photoUrl": null,
        "isOnline": true,
        "lastSeenAt": null
      },
      "lastMessage": {
        "content": "Is the book still available?",
        "createdAt": "2026-09-07T10:30:00.000Z"
      },
      "lastMessageAt": "2026-09-07T10:30:00.000Z"
    }
  ],
  "message": "Chats retrieved successfully"
}
```

---

# 32. Create or Get Chat

### POST

```text
/api/chats
```

Authentication:

```text
Required
```

### Request

```json
{
  "bookId": "BOOK_ID"
}
```

The backend determines:

```text
buyerId = authenticated user
sellerId = book.sellerId
```

The client must not be allowed to specify an arbitrary seller ID.

---

# 33. Create/Get Chat Response

```json
{
  "success": true,
  "data": {
    "id": "CHAT_ID",
    "buyerId": "BUYER_ID",
    "sellerId": "SELLER_ID",
    "bookId": "BOOK_ID",
    "createdAt": "2026-09-07T10:00:00.000Z",
    "updatedAt": "2026-09-07T10:00:00.000Z"
  },
  "message": "Chat ready"
}
```

If a chat already exists, the existing chat should be returned.

---

# 34. Get Chat Details

### GET

```text
/api/chats/:chatId
```

Authentication:

```text
Required
```

Authorization:

```text
User must be buyer or seller.
```

---

# 35. Get Messages

### GET

```text
/api/chats/:chatId/messages
```

Authentication:

```text
Required
```

Query:

```text
limit
before
```

Example:

```text
/api/chats/CHAT_ID/messages?limit=50
```

---

# 36. Message Response

```json
{
  "success": true,
  "data": [
    {
      "id": "MESSAGE_ID",
      "chatId": "CHAT_ID",
      "senderId": "USER_ID",
      "content": "Is this book still available?",
      "messageType": "text",
      "createdAt": "2026-09-07T10:30:00.000Z"
    }
  ],
  "pagination": {
    "hasMore": false
  },
  "message": "Messages retrieved successfully"
}
```

---

# 37. WebSocket

WebSocket is used for real-time chat.

REST APIs are responsible for:

```text
Chat history
Conversation creation
Initial conversation loading
```

WebSocket is responsible for:

```text
Real-time messages
Presence
Online/offline state
Last-seen updates
```

---

# 38. WebSocket Endpoint

Production:

```text
wss://<bookwala-backend-domain>/ws
```

Development:

```text
ws://localhost:<PORT>/ws
```

The exact endpoint path may be adjusted during backend implementation, but Android and backend must use one documented contract.

---

# 39. WebSocket Authentication

The WebSocket connection must be authenticated.

Conceptually:

```text
Android
 ↓
Connect
 ↓
Send Firebase ID Token
 ↓
Backend Verifies Token
 ↓
Connection Authenticated
```

An unauthenticated WebSocket connection must not access private conversations.

---

# 40. WebSocket Events

Initial event types:

```text
connection
join_chat
leave_chat
send_message
message
presence
typing
error
```

Typing indicators are optional for MVP and should only be implemented if required.

---

# 41. Join Chat Event

Client:

```json
{
  "type": "join_chat",
  "chatId": "CHAT_ID"
}
```

Backend must verify that the user is a participant.

---

# 42. Send Message Event

Client:

```json
{
  "type": "send_message",
  "chatId": "CHAT_ID",
  "content": "Is the book still available?"
}
```

Backend:

```text
Authenticate
 ↓
Validate chat
 ↓
Validate participant
 ↓
Validate content
 ↓
Store message
 ↓
Update chat.lastMessageAt
 ↓
Deliver message
```

---

# 43. Message Event

Server:

```json
{
  "type": "message",
  "data": {
    "id": "MESSAGE_ID",
    "chatId": "CHAT_ID",
    "senderId": "USER_ID",
    "content": "Yes, it is available.",
    "messageType": "text",
    "createdAt": "2026-09-07T10:35:00.000Z"
  }
}
```

---

# 44. Presence Event

Server:

```json
{
  "type": "presence",
  "data": {
    "userId": "USER_ID",
    "status": "online",
    "lastSeenAt": null
  }
}
```

Possible states:

```text
online
offline
```

When offline:

```json
{
  "type": "presence",
  "data": {
    "userId": "USER_ID",
    "status": "offline",
    "lastSeenAt": "2026-09-07T10:40:00.000Z"
  }
}
```

---

# 45. WebSocket Error

Example:

```json
{
  "type": "error",
  "data": {
    "code": "CHAT_ACCESS_DENIED",
    "message": "You are not a participant in this conversation."
  }
}
```

---

# 46. AI APIs

The BookWala AI feature is exposed through the backend.

The Android application must not call the NVIDIA API directly.

---

# 47. AI Book Search

### POST

```text
/api/ai/book-search
```

Authentication:

```text
Required
```

---

# 48. AI Request

```json
{
  "query": "I need a beginner Java programming book under 500 rupees."
}
```

---

# 49. AI Processing Flow

```mermaid
sequenceDiagram

    participant User
    participant Android
    participant BookWala as BookWala Backend
    participant NVIDIA as NVIDIA API
    participant DB as MongoDB

    User->>Android: Enter book requirement
    Android->>BookWala: POST /api/ai/book-search

    BookWala->>BookWala: Validate request
    BookWala->>NVIDIA: Understand requirement

    NVIDIA-->>BookWala: Structured requirements

    BookWala->>BookWala: Validate AI output
    BookWala->>DB: Search active books

    DB-->>BookWala: Matching listings

    BookWala->>NVIDIA: Rank/explain matches

    NVIDIA-->>BookWala: Recommendations

    BookWala->>BookWala: Validate recommended Book IDs
    BookWala-->>Android: Actual marketplace results

    Android-->>User: Display recommendations
```

---

# 50. AI Structured Requirements

The backend should normalize AI output into a controlled structure.

Example:

```json
{
  "query": "Java programming",
  "category": "Programming",
  "author": null,
  "edition": null,
  "condition": null,
  "minPrice": null,
  "maxPrice": 500
}
```

The backend must validate this structure before constructing the MongoDB query.

---

# 51. AI Search Response

```json
{
  "success": true,
  "data": {
    "query": "I need a beginner Java programming book under 500 rupees.",
    "recommendations": [
      {
        "book": {
          "id": "BOOK_ID",
          "title": "Java Programming",
          "author": "Author Name",
          "edition": "5th Edition",
          "price": 450,
          "imageUrl": "https://...",
          "sellerId": "USER_ID"
        },
        "reason": "Matches the Java subject and is below your ₹500 budget."
      }
    ]
  },
  "message": "Book recommendations generated successfully"
}
```

---

# 52. AI Result Integrity

The backend must verify that every recommended book:

```text
Exists
AND
Is active
AND
Was returned by the marketplace search
```

The AI must not be allowed to invent a listing.

---

# 53. AI No Results

Response:

```json
{
  "success": true,
  "data": {
    "query": "Very specific request",
    "recommendations": [],
    "message": "No matching books are currently available."
  },
  "message": "Search completed"
}
```

---

# 54. AI Service Failure

If NVIDIA is unavailable:

```json
{
  "success": false,
  "error": {
    "code": "AI_SERVICE_UNAVAILABLE",
    "message": "The AI assistant is temporarily unavailable. Please try again."
  }
}
```

The backend must not fabricate recommendations.

---

# 55. NVIDIA API Security

The backend stores:

```text
NVIDIA_API_KEY
NVIDIA_BASE_URL
NVIDIA_MODEL
```

The Android application never receives the API key.

Architecture:

```text
Android
 ↓
BookWala API
 ↓
NVIDIA API
```

Never:

```text
Android
 ↓
NVIDIA API
```

---

# 56. AI Rate Limiting

The AI endpoint should have rate limiting.

Reason:

- Protect NVIDIA API usage.
- Prevent abuse.
- Control cost.
- Prevent accidental request loops.

The exact limits can be configured during deployment.

---

# 57. Authentication + API Flow

```mermaid
sequenceDiagram

    participant User
    participant Android
    participant Firebase
    participant API
    participant MongoDB

    User->>Android: Login
    Android->>Firebase: Authenticate
    Firebase-->>Android: ID Token

    Android->>API: Request + Bearer Token
    API->>Firebase: Verify Token
    Firebase-->>API: Valid Identity

    API->>MongoDB: Execute Request
    MongoDB-->>API: Data

    API-->>Android: API Response
    Android-->>User: Update UI
```

---

# 58. Book Creation Flow

```mermaid
sequenceDiagram

    participant Seller
    participant Android
    participant Cloudinary
    participant API
    participant DB

    Seller->>Android: Create listing
    Android->>Cloudinary: Upload image
    Cloudinary-->>Android: Image URL + Public ID

    Android->>API: POST /api/books
    API->>API: Authenticate seller
    API->>API: Validate listing
    API->>DB: Create book

    DB-->>API: Book created
    API-->>Android: Book response
    Android-->>Seller: Listing published
```

---

# 59. Buyer Chat Flow

```mermaid
sequenceDiagram

    participant Buyer
    participant Android
    participant API
    participant DB
    participant WebSocket
    participant Seller

    Buyer->>Android: I Want to Buy
    Android->>API: POST /api/chats
    API->>DB: Find/Create chat
    DB-->>API: Chat
    API-->>Android: Chat ID

    Android->>WebSocket: Connect
    Android->>WebSocket: Join chat

    Buyer->>WebSocket: Send message
    WebSocket->>DB: Store message
    WebSocket->>Seller: Deliver message

    Seller->>WebSocket: Send reply
    WebSocket->>DB: Store reply
    WebSocket->>Buyer: Deliver reply
```

---

# 60. API Validation Rules

Every write endpoint must validate:

```text
Authentication
Authorization
Request body
Data types
Required fields
String length
Price
IDs
Resource existence
Business rules
```

Never trust Android validation alone.

---

# 61. Book Create Validation

Required:

```text
title
author
edition
price
description
imageUrl
imagePublicId
```

Example validation:

```text
title → non-empty string
author → non-empty string
edition → non-empty string
price → positive number
description → non-empty string
imageUrl → valid URL
imagePublicId → non-empty string
```

---

# 62. Book Update Validation

The backend must:

```text
Authenticate
 ↓
Find Book
 ↓
Verify Ownership
 ↓
Validate Updated Fields
 ↓
Update
```

A user must not be able to change:

```text
sellerId
```

through the update API.

---

# 63. Chat Validation

Before creating a chat:

```text
User authenticated
Book exists
Book is active
User is not the seller
```

A seller should not create a buyer conversation with themselves through the normal purchase flow.

---

# 64. Message Validation

Before sending:

```text
User authenticated
Chat exists
User is participant
Content exists
Content is within allowed length
```

---

# 65. API Security Rules

The backend must:

- Verify Firebase ID tokens.
- Validate all input.
- Enforce ownership.
- Enforce chat authorization.
- Protect NVIDIA credentials.
- Protect Cloudinary secrets.
- Rate-limit sensitive endpoints.
- Avoid leaking internal errors.
- Avoid exposing database credentials.
- Sanitize user-provided content where necessary.

---

# 66. API Logging

Backend logs may contain:

```text
Request method
Route
Status
Duration
Request ID
Error code
```

Do not log:

```text
Firebase tokens
NVIDIA API keys
Cloudinary secrets
Passwords
Private message contents unnecessarily
```

---

# 67. API Request ID

Requests should ideally receive a request identifier.

Example:

```text
X-Request-ID: abc123
```

This helps debug production errors.

---

# 68. Pagination Standard

List APIs should use:

```text
page
limit
```

Response:

```json
{
  "page": 1,
  "limit": 20,
  "totalItems": 100,
  "totalPages": 5,
  "hasNextPage": true
}
```

---

# 69. Sorting

Marketplace API may support:

```text
newest
price_asc
price_desc
```

Example:

```text
GET /api/books?sort=price_asc
```

The backend must whitelist allowed sort values.

---

# 70. Filtering

Supported filters may include:

```text
category
author
edition
condition
minPrice
maxPrice
```

Example:

```text
GET /api/books?category=programming&maxPrice=500
```

---

# 71. API Route Summary

```text
GET    /api/health

GET    /api/users/me
POST   /api/users/me
PUT    /api/users/me
GET    /api/users/:userId
GET    /api/users/me/books

GET    /api/books
POST   /api/books
GET    /api/books/:bookId
PUT    /api/books/:bookId
DELETE /api/books/:bookId
PATCH  /api/books/:bookId/status

GET    /api/search/books

GET    /api/chats
POST   /api/chats
GET    /api/chats/:chatId
GET    /api/chats/:chatId/messages

POST   /api/ai/book-search
```

---

# 72. Route Protection Summary

| Endpoint | Auth |
|---|---|
| `/api/health` | No |
| `/api/users/me` | Yes |
| `/api/users/:userId` | Optional |
| `/api/users/me/books` | Yes |
| `GET /api/books` | Optional |
| `POST /api/books` | Yes |
| `GET /api/books/:id` | Optional |
| `PUT /api/books/:id` | Yes + Owner |
| `DELETE /api/books/:id` | Yes + Owner |
| `PATCH /api/books/:id/status` | Yes + Owner |
| `/api/search/books` | Optional |
| `/api/chats` | Yes |
| `/api/chats/:id` | Yes + Participant |
| `/api/chats/:id/messages` | Yes + Participant |
| `/api/ai/book-search` | Yes |

---

# 73. Client Architecture

Android should consume APIs through:

```text
Compose UI
 ↓
ViewModel
 ↓
Use Case
 ↓
Repository
 ↓
API Client
 ↓
BookWala Backend
```

The UI should never directly construct arbitrary HTTP requests.

---

# 74. API State Handling

Android should represent:

```text
Loading
Success
Empty
Error
```

For example:

```text
Marketplace Loading
Marketplace Success
Marketplace Empty
Marketplace Error
```

The same pattern should be used throughout the application.

---

# 75. API Retry Policy

Retry only appropriate failures.

Potential retry cases:

```text
Temporary network failure
503 Service Unavailable
Transient WebSocket failure
```

Do not automatically retry:

```text
400
401
403
404
422
```

unless the user changes the request or re-authenticates where appropriate.

---

# 76. Backend Layering

Every API request should follow:

```text
Route
 ↓
Middleware
 ↓
Controller
 ↓
Service
 ↓
Repository / Model
 ↓
MongoDB
```

External services:

```text
Service
 ├── Firebase
 ├── Cloudinary
 ├── NVIDIA
 └── WebSocket
```

---

# 77. Controller Responsibilities

Controllers should:

- Receive request.
- Read validated input.
- Call service.
- Return response.
- Map known errors to HTTP status codes.

Controllers should not contain large amounts of business logic.

---

# 78. Service Responsibilities

Services should contain business rules.

Examples:

```text
BookService
ChatService
MessageService
UserService
AIService
```

---

# 79. Repository Responsibilities

Repositories handle persistence operations where a repository abstraction is used.

Examples:

```text
BookRepository
UserRepository
ChatRepository
MessageRepository
```

---

# 80. Final Backend Architecture

```mermaid
flowchart TB

    CLIENT[Android<br/>Kotlin + Compose]

    ROUTER[Express Routes]

    AUTH[Firebase Auth Middleware]

    CONTROLLER[Controllers]

    SERVICE[Services]

    DB[(MongoDB)]

    CLOUD[Cloudinary]

    NVIDIA[NVIDIA API]

    WS[WebSocket Server]

    CLIENT --> ROUTER
    CLIENT <--> WS

    ROUTER --> AUTH
    AUTH --> CONTROLLER
    CONTROLLER --> SERVICE

    SERVICE --> DB
    SERVICE --> CLOUD
    SERVICE --> NVIDIA

    WS --> AUTH
    WS --> DB
```

---

# 81. Complete BookWala API Flow

```mermaid
flowchart TD

    START[Android App] --> AUTH[Firebase Authentication]

    AUTH --> TOKEN[Firebase ID Token]

    TOKEN --> API[BookWala REST API]

    API --> MARKET[Marketplace]

    MARKET --> BOOKS[(MongoDB Books)]

    MARKET --> SEARCH[Search / Filter]

    SEARCH --> BOOKS

    BOOKS --> DETAILS[Book Details]

    DETAILS --> BUY[I Want to Buy]

    BUY --> CHATAPI[Chat API]

    CHATAPI --> CHATDB[(MongoDB Chats)]

    CHATAPI --> WS[WebSocket]

    WS --> MESSAGE[Real-Time Messages]

    MESSAGE --> MESSAGE_DB[(MongoDB Messages)]

    MESSAGE --> PRESENCE[Online / Offline / Last Seen]

    AUTH --> SELLER[Create Listing]

    SELLER --> CLOUD[Cloudinary]

    CLOUD --> IMAGE[Image URL]

    IMAGE --> API

    API --> BOOKS

    AUTH --> AI[AI Assistant]

    AI --> AIAPI[/api/ai/book-search]

    AIAPI --> NVIDIA[NVIDIA API]

    NVIDIA --> REQUIREMENTS[Structured Requirements]

    REQUIREMENTS --> SEARCHAI[Controlled Marketplace Search]

    SEARCHAI --> BOOKS

    BOOKS --> RESULTS[Actual Book Results]

    RESULTS --> AI
    AI --> DETAILS
```

---

# 82. Development Contract

The API specification must be implemented incrementally.

For each endpoint:

```text
Define
 ↓
Implement
 ↓
Validate
 ↓
Test
 ↓
Integrate Android
 ↓
Test End-to-End
 ↓
Approve
 ↓
Commit
```

Do not implement a large number of endpoints without testing them.

---

# 83. API Testing Requirements

Every implemented endpoint should be tested for:

```text
Success
Invalid input
Missing authentication
Invalid authentication
Unauthorized access
Resource not found
Database failure
External service failure
```

For protected resources, test both:

```text
Authorized user
Unauthorized user
```

---

# 84. MVP API Priority

Recommended implementation order:

```text
1. Health Check
        ↓
2. Firebase Authentication Middleware
        ↓
3. User Profile
        ↓
4. Create Book
        ↓
5. Get Books
        ↓
6. Get Book Details
        ↓
7. Update/Delete Book
        ↓
8. Search / Filter
        ↓
9. Create/Get Chat
        ↓
10. WebSocket Messaging
        ↓
11. Online / Offline / Last Seen
        ↓
12. AI Book Search
```

---

# 85. Final API Principle

BookWala follows one central rule:

```text
Android requests
        ↓
Backend validates
        ↓
Backend authorizes
        ↓
Backend performs business logic
        ↓
Backend accesses data/services
        ↓
Backend returns trusted result
```

The Android application is the client.

The backend is the authority.

MongoDB is the marketplace data source.

Firebase is the authentication identity provider.

Cloudinary is the image storage provider.

WebSocket is the real-time communication layer.

NVIDIA is the AI reasoning/discovery provider.

No external service should bypass the BookWala backend's security and business rules.