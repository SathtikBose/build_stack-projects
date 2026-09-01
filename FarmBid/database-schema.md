# FarmBid – Database Schema

**Project Name:** FarmBid **Package Name:** `com.buildstack.farmbid` **Database:** MongoDB Atlas **ODM:** Mongoose **Document Version:** 1.0 **Status:** MVP

---

# 1. Database Overview

FarmBid uses MongoDB as its primary database.

The database stores:

- User profiles.
- Product listings.
- Product inventory.
- Buyer bids.
- Deals created from accepted bids.
- Chats.
- Messages.

Authentication credentials are managed by Firebase Authentication.

MongoDB does not store:

- User passwords.
- Google passwords.
- Firebase authentication tokens.

MongoDB stores the Firebase user identifier (`firebaseUid`) and application-specific user profile information.

---

# 2. Database Collections

```text
farmbid
│
├── users
├── products
├── bids
├── deals
├── chats
└── messages
```

---

# 3. Collection Relationships

```text
USER
 │
 ├── FARMER
 │      ├── PRODUCTS
 │      ├── RECEIVED BIDS
 │      └── DEALS
 │
 └── BUYER
        ├── PLACED BIDS
        └── DEALS

PRODUCT
   │
   └── BIDS
         │
         └── ACCEPTED BID
                  │
                  ▼
                 DEAL
                  │
                  ▼
                 CHAT
                  │
                  ▼
               MESSAGES
```

---

# 4. Users Collection

**Collection Name:** `users`

Stores application-specific user profile information.

Authentication is handled separately by Firebase Authentication.

---

## 4.1 User Document

```json
{
  "_id": "ObjectId",
  "firebaseUid": "firebase-user-uid",
  "role": "FARMER",
  "fullName": "Rahul Kumar",
  "email": "rahul@example.com",
  "phoneNumber": "+919876543210",
  "profileImage": {
    "url": "https://example.com/profile.jpg",
    "publicId": "farmbid/profiles/user-123"
  },
  "language": "en",
  "location": {
    "address": "Jamshedpur, Jharkhand",
    "latitude": 22.8046,
    "longitude": 86.2029
  },
  "isProfileComplete": true,
  "createdAt": "2026-08-30T00:00:00.000Z",
  "updatedAt": "2026-08-30T00:00:00.000Z"
}
```

---

## 4.2 User Fields

| Field               | Type     | Required | Rules              |
| ------------------- | -------- | -------- | ------------------ |
| `_id`               | ObjectId | Yes      | Primary ID         |
| `firebaseUid`       | String   | Yes      | Unique             |
| `role`              | String   | Yes      | FARMER or BUYER    |
| `fullName`          | String   | Yes      | 2–100 characters   |
| `email`             | String   | Yes      | Valid email        |
| `phoneNumber`       | String   | No       | Valid phone format |
| `profileImage`      | Object   | No       | Profile image      |
| `language`          | String   | Yes      | `en` or `hi`       |
| `location`          | Object   | No       | User location      |
| `isProfileComplete` | Boolean  | Yes      | Default false      |
| `createdAt`         | Date     | Yes      | Automatic          |
| `updatedAt`         | Date     | Yes      | Automatic          |

---

## 4.3 User Role Values

```text
FARMER
BUYER
```

---

## 4.4 User Indexes

```text
firebaseUid: unique
email: unique
role: index
```

---

# 5. Products Collection

**Collection Name:** `products`

Stores agricultural products listed by farmers.

Only users with the `FARMER` role can create products.

---

## 5.1 Product Document

```json
{
  "_id": "ObjectId",
  "farmerId": "ObjectId",
  "name": "Wheat",
  "quantity": 20,
  "unit": "KG",
  "basePrice": 30,
  "location": {
    "address": "Jamshedpur, Jharkhand",
    "latitude": 22.8046,
    "longitude": 86.2029
  },
  "harvestDate": "2026-08-20T00:00:00.000Z",
  "quality": "Grade A",
  "images": [
    {
      "url": "https://res.cloudinary.com/example/image1.jpg",
      "publicId": "farmbid/products/product-1-image-1"
    },
    {
      "url": "https://res.cloudinary.com/example/image2.jpg",
      "publicId": "farmbid/products/product-1-image-2"
    }
  ],
  "status": "AVAILABLE",
  "acceptedBidId": null,
  "createdAt": "2026-08-30T00:00:00.000Z",
  "updatedAt": "2026-08-30T00:00:00.000Z"
}
```

---

## 5.2 Product Fields

| Field           | Type     | Required | Rules                 |
| --------------- | -------- | -------- | --------------------- |
| `_id`           | ObjectId | Yes      | Primary ID            |
| `farmerId`      | ObjectId | Yes      | References User       |
| `name`          | String   | Yes      | 2–100 characters      |
| `quantity`      | Number   | Yes      | Greater than 0        |
| `unit`          | String   | Yes      | Supported unit        |
| `basePrice`     | Number   | Yes      | Greater than 0        |
| `location`      | Object   | Yes      | Product location      |
| `harvestDate`   | Date     | Yes      | Cannot be future date |
| `quality`       | String   | Yes      | Quality description   |
| `images`        | Array    | Yes      | Minimum 2, maximum 4  |
| `status`        | String   | Yes      | Product status        |
| `acceptedBidId` | ObjectId | No       | Accepted bid          |
| `createdAt`     | Date     | Yes      | Automatic             |
| `updatedAt`     | Date     | Yes      | Automatic             |

---

## 5.3 Product Units

```text
KG
QUINTAL
TON
```

---

## 5.4 Product Status

```text
AVAILABLE
BIDDING_CLOSED
DEAL_CREATED
SOLD
CANCELLED
```

---

## 5.5 Product Indexes

```text
farmerId
status
name
createdAt
location.address
```

---

# 6. Bids Collection

**Collection Name:** `bids`

Stores offers submitted by buyers.

A product can receive multiple bids.

Only one bid can be accepted.

---

## 6.1 Bid Document

```json
{
  "_id": "ObjectId",
  "productId": "ObjectId",
  "buyerId": "ObjectId",
  "farmerId": "ObjectId",
  "offeredPrice": 35,
  "requestedQuantity": 20,
  "status": "PENDING",
  "createdAt": "2026-08-30T00:00:00.000Z",
  "updatedAt": "2026-08-30T00:00:00.000Z"
}
```

---

## 6.2 Bid Fields

| Field               | Type     | Required | Rules              |
| ------------------- | -------- | -------- | ------------------ |
| `_id`               | ObjectId | Yes      | Primary ID         |
| `productId`         | ObjectId | Yes      | References Product |
| `buyerId`           | ObjectId | Yes      | References User    |
| `farmerId`          | ObjectId | Yes      | References User    |
| `offeredPrice`      | Number   | Yes      | Greater than 0     |
| `requestedQuantity` | Number   | Yes      | Greater than 0     |
| `status`            | String   | Yes      | Bid status         |
| `createdAt`         | Date     | Yes      | Automatic          |
| `updatedAt`         | Date     | Yes      | Automatic          |

---

## 6.3 Bid Status

```text
PENDING
ACCEPTED
REJECTED
CANCELLED
EXPIRED
```

---

## 6.4 Bid Indexes

```text
productId
buyerId
farmerId
status
createdAt
```

Recommended compound index:

```text
productId + buyerId
```

This prevents duplicate active bids by the same buyer for the same product.

---

# 7. Deals Collection

**Collection Name:** `deals`

A deal is created when a farmer accepts a buyer's bid.

One accepted bid creates one deal.

---

## 7.1 Deal Document

```json
{
  "_id": "ObjectId",
  "productId": "ObjectId",
  "farmerId": "ObjectId",
  "buyerId": "ObjectId",
  "acceptedBidId": "ObjectId",
  "finalPrice": 35,
  "quantity": 20,
  "status": "ACTIVE",
  "createdAt": "2026-08-30T00:00:00.000Z",
  "completedAt": null,
  "cancelledAt": null
}
```

---

## 7.2 Deal Fields

| Field           | Type     | Required | Rules                  |
| --------------- | -------- | -------- | ---------------------- |
| `_id`           | ObjectId | Yes      | Primary ID             |
| `productId`     | ObjectId | Yes      | References Product     |
| `farmerId`      | ObjectId | Yes      | References User        |
| `buyerId`       | ObjectId | Yes      | References User        |
| `acceptedBidId` | ObjectId | Yes      | References Bid         |
| `finalPrice`    | Number   | Yes      | Accepted bid price     |
| `quantity`      | Number   | Yes      | Agreed quantity        |
| `status`        | String   | Yes      | Deal status            |
| `createdAt`     | Date     | Yes      | Automatic              |
| `completedAt`   | Date     | No       | Completion timestamp   |
| `cancelledAt`   | Date     | No       | Cancellation timestamp |

---

## 7.3 Deal Status

```text
ACTIVE
COMPLETED
CANCELLED
```

---

## 7.4 Deal Indexes

```text
productId: unique
farmerId
buyerId
acceptedBidId: unique
status
```

---

# 8. Chats Collection

**Collection Name:** `chats`

A chat is created automatically when a deal is created.

Each deal has one chat.

---

## 8.1 Chat Document

```json
{
  "_id": "ObjectId",
  "farmerId": "ObjectId",
  "buyerId": "ObjectId",
  "dealId": "ObjectId",
  "lastMessage": "Hello, when can we meet?",
  "lastMessageAt": "2026-08-30T10:30:00.000Z",
  "createdAt": "2026-08-30T00:00:00.000Z",
  "updatedAt": "2026-08-30T10:30:00.000Z"
}
```

---

## 8.2 Chat Fields

| Field           | Type     | Required | Rules               |
| --------------- | -------- | -------- | ------------------- |
| `_id`           | ObjectId | Yes      | Primary ID          |
| `farmerId`      | ObjectId | Yes      | References User     |
| `buyerId`       | ObjectId | Yes      | References User     |
| `dealId`        | ObjectId | Yes      | Unique              |
| `lastMessage`   | String   | No       | Latest message      |
| `lastMessageAt` | Date     | No       | Latest message time |
| `createdAt`     | Date     | Yes      | Automatic           |
| `updatedAt`     | Date     | Yes      | Automatic           |

---

# 9. Messages Collection

**Collection Name:** `messages`

Stores chat messages.

Messages are stored separately from chats to avoid large chat documents.

---

## 9.1 Message Document

```json
{
  "_id": "ObjectId",
  "chatId": "ObjectId",
  "senderId": "ObjectId",
  "content": "Hello, I am interested in buying the wheat.",
  "messageType": "TEXT",
  "isRead": false,
  "createdAt": "2026-08-30T10:30:00.000Z",
  "updatedAt": "2026-08-30T10:30:00.000Z"
}
```

---

## 9.2 Message Fields

| Field         | Type     | Required | Rules             |
| ------------- | -------- | -------- | ----------------- |
| `_id`         | ObjectId | Yes      | Primary ID        |
| `chatId`      | ObjectId | Yes      | References Chat   |
| `senderId`    | ObjectId | Yes      | References User   |
| `content`     | String   | Yes      | 1–2000 characters |
| `messageType` | String   | Yes      | TEXT              |
| `isRead`      | Boolean  | Yes      | Default false     |
| `createdAt`   | Date     | Yes      | Automatic         |
| `updatedAt`   | Date     | Yes      | Automatic         |

---

## 9.3 Message Indexes

Recommended:

```text
chatId + createdAt
senderId
```

---

# 10. Entity Relationship Summary

```text
USERS
  │
  ├── 1:N → PRODUCTS
  │
  ├── 1:N → BIDS
  │
  ├── 1:N → DEALS
  │
  └── 1:N → MESSAGES

PRODUCTS
  │
  ├── N:1 → USERS (Farmer)
  │
  ├── 1:N → BIDS
  │
  └── 1:1 → DEALS

BIDS
  │
  ├── N:1 → PRODUCTS
  │
  ├── N:1 → USERS (Buyer)
  │
  └── 1:0..1 → DEAL

DEALS
  │
  ├── 1:1 → ACCEPTED BID
  │
  └── 1:1 → CHAT

CHATS
  │
  └── 1:N → MESSAGES
```

---

# 11. Database Consistency Rules

The following rules must be enforced by the backend.

## Product

- Only farmers can create products.
- A product must have 2 to 4 images.
- Quantity must be greater than zero.
- Harvest date cannot be in the future.
- Only the owner can update or delete a product.

## Bid

- Only buyers can place bids.
- A buyer cannot bid on their own product.
- Product must have `AVAILABLE` status.
- Requested quantity cannot exceed available quantity.
- Offered price must be greater than zero.
- Only one bid can be accepted.
- Accepted bids cannot be edited.

## Deal

- A deal is created only from an accepted bid.
- A product can have only one active deal.
- A deal can be accessed only by the farmer and buyer.

## Chat

- A chat is created from a deal.
- Only deal participants can access the chat.
- Only deal participants can send messages.

---

# 12. Database Operation Flow

## Product Creation

```text
Farmer
  ↓
Create Product
  ↓
Validate Product Data
  ↓
Validate 2–4 Images
  ↓
Upload Images to Cloudinary
  ↓
Receive Secure URLs
  ↓
Create Product in MongoDB
```

---

## Bid Acceptance

```text
Farmer
  ↓
Accept Bid
  ↓
Verify Product Ownership
  ↓
Verify Bid is Pending
  ↓
Transaction / Atomic Update
  ↓
Bid Status = ACCEPTED
  ↓
Create Deal
  ↓
Create Chat
  ↓
Other Pending Bids = REJECTED
  ↓
Product Status = DEAL_CREATED
```

This operation should use a MongoDB transaction where supported to prevent multiple bids from being accepted simultaneously.

---

# 13. Database Naming Conventions

## Collections

Use lowercase plural names:

```text
users
products
bids
deals
chats
messages
```

## Fields

Use camelCase:

```text
firebaseUid
farmerId
basePrice
harvestDate
createdAt
updatedAt
```

---

# 14. Database Backup and Recovery

For development and MVP deployment:

- MongoDB Atlas managed backups should be enabled when available.
- Production credentials must never be committed to GitHub.
- MongoDB connection strings must be stored in environment variables.

---

# 15. Final Database Summary

```text
MongoDB
│
├── users
│   └── Application user profiles
│
├── products
│   └── Farmer agricultural listings
│
├── bids
│   └── Buyer offers
│
├── deals
│   └── Accepted bid agreements
│
├── chats
│   └── Deal conversations
│
└── messages
    └── Chat messages
```

The database is designed to support the FarmBid MVP while keeping the data model simple and scalable.
