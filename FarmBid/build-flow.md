# FarmBid – Complete Build Flow

**Project Name:** FarmBid **Package Name:** `com.buildstack.farmbid` **Build Strategy:** Phase-by-Phase Development **UI/UX Tool:** Google Stitch **Version:** 1.0

---

# 1. Build Philosophy

FarmBid must not be built as one large implementation.

The project will be developed in small, stable phases.

Every phase follows:

```text
PLAN
  ↓
DESIGN
  ↓
BUILD
  ↓
TEST
  ↓
FIX
  ↓
VERIFY
  ↓
COMMIT
```

No phase should move forward with unresolved build errors.

---

# 2. Complete Development Flow

```text
PROJECT DOCUMENTATION
        ↓
PROJECT SETUP
        ↓
BACKEND FOUNDATION
        ↓
ANDROID FOUNDATION
        ↓
AUTHENTICATION
        ↓
USER PROFILE
        ↓
PRODUCT SYSTEM
        ↓
MARKETPLACE
        ↓
BIDDING SYSTEM
        ↓
DEAL SYSTEM
        ↓
CHAT SYSTEM
        ↓
MULTI-LANGUAGE SUPPORT
        ↓
FULL INTEGRATION TESTING
        ↓
BUG FIXING
        ↓
DEPLOYMENT
        ↓
FINAL TESTING
```

---

# PHASE 0 – PROJECT PREPARATION

## Objective

Prepare the complete project structure before building features.

---

## Step 1: Verify Documentation

Confirm the existence of:

```text
prd.md
trd.md
database-schema.md
endpoints.md
ui-ux-brief.md
instruction.md
build-flow.md
```

Review for conflicts.

---

## Step 2: Create Git Repository

Create:

```text
FarmBid/
```

Initialize:

```bash
git init
```

Add:

```text
README.md
.gitignore
docs/
android/
backend/
```

---

## Step 3: Initial Commit

Commit message:

```text
chore: initialize FarmBid project structure
```

---

# PHASE 1 – BACKEND FOUNDATION

## Objective

Create the Node.js backend foundation.

---

## Build

Create:

```text
Express server
MongoDB connection
Environment configuration
CORS
Global error handling
Authentication middleware structure
```

Required structure:

```text
backend/
├── src/
│   ├── config/
│   ├── controllers/
│   ├── middleware/
│   ├── models/
│   ├── routes/
│   ├── services/
│   └── utils/
│
├── package.json
├── server.js
└── .env.example
```

---

## Test

Verify:

```text
Server starts successfully
MongoDB connects
Health endpoint works
Invalid routes return proper errors
```

Test:

```http
GET /health
```

---

## Commit

```text
feat: initialize backend foundation
```

---

# PHASE 2 – ANDROID FOUNDATION

## Objective

Create the Android application foundation.

---

## Build

Create:

```text
Kotlin project
Jetpack Compose
Navigation
MVVM structure
Hilt dependency injection
Retrofit
Base theme
```

Package:

```text
com.buildstack.farmbid
```

---

## Test

Run:

```bash
./gradlew build
```

Verify:

```text
Application builds successfully
Application launches
Navigation works
No crash on startup
```

---

## Commit

```text
feat: initialize Android application foundation
```

---

# PHASE 3 – AUTHENTICATION

## Objective

Implement user authentication.

---

## Design

Create the following screens in Google Stitch:

```text
Splash
Login
Sign Up
Forgot Password
Role Selection
```

Review and approve the designs.

---

## Build

Implement:

```text
Email Registration
Email Login
Google Sign-In
Forgot Password
Logout
Firebase Authentication
```

---

## Backend

Implement:

```text
Firebase token verification
Authenticated user middleware
```

---

## Test

Test:

```text
New account registration
Existing user login
Invalid password
Google login
Forgot password
Logout
Invalid token
Expired token
```

Run:

```bash
./gradlew build
```

---

## Commit

```text
feat: implement user authentication
```

---

# PHASE 4 – USER PROFILE

## Objective

Create FarmBid user profiles.

---

## Design

Create in Google Stitch:

```text
Profile Setup
Profile
Edit Profile
```

---

## Build

Implement:

```text
Farmer role
Buyer role
Profile image
Name
Phone number
Location
Language
```

---

## Backend

Implement:

```text
POST /api/users/profile
GET /api/users/me
PUT /api/users/me
GET /api/users/:id
```

---

## Test

Test:

```text
Create farmer profile
Create buyer profile
Update profile
Role validation
Location update
Unauthorized access
```

Run:

```bash
./gradlew build
```

---

## Commit

```text
feat: implement user profile management
```

---

# PHASE 5 – PRODUCT SYSTEM

## Objective

Allow farmers to create and manage product listings.

---

## Design

Create in Google Stitch:

```text
Farmer Home
Add Product
My Products
Product Edit
```

---

## Build

Product fields:

```text
Product Name
Quantity
Unit
Base Price
Location
Harvest Date
Quality
2–4 Images
```

---

## Image Upload

Flow:

```text
ANDROID
   ↓
SELECT IMAGES
   ↓
BACKEND
   ↓
CLOUDINARY
   ↓
MONGODB
```

---

## Backend

Implement:

```text
POST /api/products
GET /api/products/my-products
PUT /api/products/:id
DELETE /api/products/:id
```

---

## Test

Test:

```text
Create product
Upload 2 images
Upload 4 images
Reject 1 image
Reject 5 images
Reject future harvest date
Update product
Delete product
Unauthorized user
```

Run:

```bash
./gradlew build
```

---

## Commit

```text
feat: implement farmer product management
```

---

# PHASE 6 – MARKETPLACE

## Objective

Allow users to discover products.

---

## Design

Create in Google Stitch:

```text
Buyer Home
Marketplace
Search
Product Details
```

---

## Build

Implement:

```text
Product list
Search
Location filter
Price filter
Sorting
Pagination
Product details
```

---

## Backend

Implement:

```text
GET /api/products
GET /api/products/:id
```

---

## Test

Test:

```text
Load marketplace
Search product
Filter products
Open product
Pagination
Empty state
Network error
```

Run:

```bash
./gradlew build
```

---

## Commit

```text
feat: implement marketplace product browsing
```

---

# PHASE 7 – BIDDING SYSTEM

## Objective

Implement the core FarmBid bidding mechanic.

---

## Design

Create in Google Stitch:

```text
Place Bid
Bid Confirmation
My Bids
Received Bids
Bid Comparison
Accept Bid Confirmation
```

---

## Build

Buyer can:

```text
Place Bid
Update Bid
Cancel Bid
View Bid Status
```

Farmer can:

```text
View Bids
Compare Bids
Accept Bid
Reject Bid
```

---

## Backend

Implement:

```text
POST /api/products/:productId/bids
GET /api/products/:productId/bids
GET /api/bids/my-bids
PUT /api/bids/:id
POST /api/bids/:id/cancel
POST /api/bids/:id/accept
POST /api/bids/:id/reject
```

---

## Critical Test

Test:

```text
Buyer cannot bid on own product
Buyer cannot exceed available quantity
Buyer can cancel pending bid
Farmer can accept bid
Farmer can reject bid
Only one bid can be accepted
Other pending bids are rejected
Duplicate acceptance is prevented
```

---

## Integration Test

```text
Farmer Creates Product
        ↓
Buyer A Places Bid
        ↓
Buyer B Places Bid
        ↓
Farmer Views Bids
        ↓
Farmer Accepts One Bid
        ↓
Deal Created
        ↓
Other Bid Rejected
```

---

## Commit

```text
feat: implement product bidding system
```

---

# PHASE 8 – DEAL SYSTEM

## Objective

Create the agreement between farmer and buyer.

---

## Design

Create in Google Stitch:

```text
Deals List
Deal Details
Complete Deal Confirmation
Cancel Deal Confirmation
```

---

## Build

A deal includes:

```text
Product
Farmer
Buyer
Accepted Price
Quantity
Status
```

---

## Backend

Implement:

```text
GET /api/deals
GET /api/deals/:id
POST /api/deals/:id/complete
POST /api/deals/:id/cancel
```

---

## Test

Test:

```text
Deal creation
Only participants can access deal
Complete deal
Cancel deal
Product status update
Unauthorized access
```

Run:

```bash
./gradlew build
```

---

## Commit

```text
feat: implement deal management system
```

---

# PHASE 9 – CHAT SYSTEM

## Objective

Allow farmer and buyer to communicate after a deal is created.

---

## Design

Create in Google Stitch:

```text
Chat List
Chat Screen
```

---

## Build

Use:

```text
REST API
+
POLLING
```

Polling interval:

```text
5–10 seconds
```

---

## Backend

Implement:

```text
GET /api/chats
GET /api/chats/:id
GET /api/chats/:id/messages
POST /api/chats/:id/messages
POST /api/chats/:id/read
```

---

## Test

Test:

```text
Chat created after accepted bid
Only participants can access chat
Send message
Receive message
Refresh messages
Read messages
Stop polling when screen closes
```

Run:

```bash
./gradlew build
```

---

## Commit

```text
feat: implement deal-based chat system
```

---

# PHASE 10 – MULTI-LANGUAGE SUPPORT

## Objective

Support English and Hindi.

---

## Build

Add:

```text
English
Hindi
```

Do not leave user-facing strings hardcoded.

---

## Test

Test:

```text
English UI
Hindi UI
Language switching
Long text layout
Button text
Error messages
```

Run:

```bash
./gradlew build
```

---

## Commit

```text
feat: add English and Hindi language support
```

---

# PHASE 11 – FULL INTEGRATION TESTING

## Objective

Test the entire FarmBid application.

---

## Complete Farmer Flow

```text
Register
  ↓
Login
  ↓
Select Farmer Role
  ↓
Create Profile
  ↓
Add Product
  ↓
Upload Images
  ↓
Publish Product
  ↓
Receive Bids
  ↓
Accept Bid
  ↓
Deal Created
  ↓
Chat With Buyer
  ↓
Complete Deal
```

---

## Complete Buyer Flow

```text
Register
  ↓
Login
  ↓
Select Buyer Role
  ↓
Create Profile
  ↓
Browse Products
  ↓
Open Product
  ↓
Place Bid
  ↓
Bid Accepted
  ↓
Deal Created
  ↓
Chat With Farmer
  ↓
Complete Deal
```

---

# PHASE 12 – ERROR AND EDGE CASE TESTING

Test:

```text
No internet
Slow network
Backend unavailable
Invalid Firebase token
Expired Firebase token
MongoDB unavailable
Cloudinary upload failure
Image upload failure
Duplicate bid acceptance
Invalid product data
Unauthorized role
Unauthorized resource access
```

Fix all critical errors.

---

# PHASE 13 – FINAL BUILD

## Android

Run:

```bash
./gradlew build
```

The build must complete successfully.

---

## Backend

Run all available API and integration tests.

Verify:

```text
No startup errors
MongoDB connection
Firebase configuration
Cloudinary configuration
CORS configuration
```

---

# PHASE 14 – DEPLOYMENT

## Backend Deployment

Deploy the Node.js backend to Render.

Configure:

```text
MongoDB URI
Firebase configuration
Cloudinary configuration
Allowed CORS origin
Environment variables
```

---

## Production Test

Test:

```text
Authentication
Profile
Product creation
Image upload
Marketplace
Bidding
Deals
Chat
```

---

# PHASE 15 – FINAL GIT CHECK

Before final project completion:

```bash
git status
```

Verify:

- No API secrets.
- No Firebase service account files.
- No `.env` files.
- No unnecessary build files.
- No incomplete code.

---

# Final Project Flow

```text
DOCUMENTATION
        ↓
PROJECT SETUP
        ↓
BACKEND FOUNDATION
        ↓
ANDROID FOUNDATION
        ↓
AUTHENTICATION
        ↓
USER PROFILE
        ↓
PRODUCT MANAGEMENT
        ↓
MARKETPLACE
        ↓
BIDDING
        ↓
DEALS
        ↓
CHAT
        ↓
LANGUAGES
        ↓
INTEGRATION TESTING
        ↓
EDGE CASE TESTING
        ↓
GRADLE BUILD
        ↓
DEPLOYMENT
        ↓
PRODUCTION TEST
        ↓
FINAL GIT COMMIT
```

---

# Mandatory Rule for Every Phase

```text
BUILD
  ↓
TEST
  ↓
FIX
  ↓
TEST AGAIN
  ↓
VERIFY
  ↓
COMMIT
```

Do not move to the next phase until the current phase is stable.
