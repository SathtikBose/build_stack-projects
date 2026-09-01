# FarmBid – Technical Requirements Document (TRD)

**Project Name:** FarmBid **Android Package Name:** `com.buildstack.farmbid` **Platform:** Android **Application Type:** Agricultural B2B Marketplace **Document Version:** 1.0 **Status:** MVP / College Project **Last Updated:** August 30, 2026

---

# 1. Purpose

This Technical Requirements Document (TRD) defines the technical architecture, technology stack, modules, data structures, API requirements, security requirements, deployment strategy, testing requirements, and development workflow for FarmBid.

FarmBid is an Android application that connects farmers with buyers for bulk agricultural product transactions.

The core application flow is:

```text
Farmer Lists Product
        ↓
Buyer Browses Product
        ↓
Buyer Places Bid
        ↓
Farmer Reviews Bids
        ↓
Farmer Accepts One Bid
        ↓
Deal Created
        ↓
Users Communicate
        ↓
Offline Transaction Completed
```

FarmBid does not process payments or transportation within the MVP.

---

# 2. System Architecture

FarmBid will use a client-server architecture.

```text
┌──────────────────────────────┐
│      Android Application     │
│           Kotlin             │
│          MVVM                │
└──────────────┬───────────────┘
               │
               │ HTTPS / REST
               ▼
┌──────────────────────────────┐
│       Node.js Backend        │
│          Express.js          │
│                              │
│  ┌────────────────────────┐  │
│  │ Firebase Token Verify  │  │
│  ├────────────────────────┤  │
│  │ Business Logic         │  │
│  ├────────────────────────┤  │
│  │ REST API               │  │
│  └────────────────────────┘  │
└──────┬───────────┬───────────┘
       │           │
       ▼           ▼
┌─────────────┐ ┌──────────────┐
│   MongoDB   │ │  Cloudinary  │
│  Database   │ │ Image Storage│
└─────────────┘ └──────────────┘

       │
       ▼
┌──────────────────────────────┐
│ Firebase Authentication      │
│ Email / Password             │
│ Google Sign-In               │
│ Password Reset               │
└──────────────────────────────┘
```

---

# 3. Technology Stack

## 3.1 Android Application

| Component            | Technology                    |
| -------------------- | ----------------------------- |
| Language             | Kotlin                        |
| Package Name         | `com.buildstack.farmbid`      |
| Architecture         | MVVM                          |
| UI                   | Jetpack Compose               |
| Networking           | Retrofit                      |
| JSON Parsing         | Kotlin Serialization          |
| Async Programming    | Kotlin Coroutines             |
| Dependency Injection | Hilt                          |
| Local Preferences    | DataStore                     |
| Navigation           | Jetpack Navigation Compose    |
| Authentication       | Firebase Authentication       |
| Google Login         | Firebase Google Sign-In       |
| Image Loading        | Coil                          |
| Location             | Google Play Services Location |
| Language             | Android Resource Localization |

---

## 3.2 Backend

| Component             | Technology         |
| --------------------- | ------------------ |
| Runtime               | Node.js            |
| Framework             | Express.js         |
| API Type              | REST               |
| Language              | JavaScript         |
| Authentication        | Firebase Admin SDK |
| Database              | MongoDB            |
| ODM                   | Mongoose           |
| Validation            | Zod                |
| Environment Variables | dotenv             |
| Logging               | Morgan             |
| Security              | Helmet             |
| CORS                  | cors               |

---

## 3.3 Cloud Services

| Service                 | Purpose               |
| ----------------------- | --------------------- |
| Firebase Authentication | User authentication   |
| Firebase Google Auth    | Google Sign-In        |
| Firebase Password Reset | Password reset email  |
| MongoDB Atlas           | Cloud database        |
| Cloudinary              | Product image storage |
| Render                  | Backend deployment    |
| GitHub                  | Source control        |

---

# 4. Android Architecture

The Android application will follow MVVM architecture.

```text
UI
│
├── Screen
│
├── ViewModel
│
├── Use Case
│
├── Repository
│
└── API / Firebase
```

The preferred flow is:

```text
Compose Screen
        ↓
ViewModel
        ↓
Use Case
        ↓
Repository
        ↓
Retrofit API
        ↓
Node.js Backend
```

---

# 5. Android Package Structure

```text
com.buildstack.farmbid
│
├── core
│   ├── common
│   ├── network
│   ├── navigation
│   ├── ui
│   └── util
│
├── data
│   ├── local
│   ├── remote
│   │   ├── api
│   │   ├── dto
│   │   └── interceptor
│   └── repository
│
├── domain
│   ├── model
│   ├── repository
│   └── usecase
│
├── feature
│   ├── auth
│   ├── profile
│   ├── marketplace
│   ├── product
│   ├── bid
│   ├── deal
│   ├── chat
│   └── settings
│
├── di
│
└── MainActivity.kt
```

---

# 6. Backend Architecture

The backend will follow a layered architecture.

```text
Request
   ↓
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

---

# 7. Backend Project Structure

```text
backend
│
├── src
│   ├── config
│   │   ├── db.js
│   │   ├── cloudinary.js
│   │   └── firebase.js
│   │
│   ├── controllers
│   │   ├── user.controller.js
│   │   ├── product.controller.js
│   │   ├── bid.controller.js
│   │   ├── deal.controller.js
│   │   └── chat.controller.js
│   │
│   ├── services
│   │   ├── user.service.js
│   │   ├── product.service.js
│   │   ├── bid.service.js
│   │   ├── deal.service.js
│   │   └── chat.service.js
│   │
│   ├── models
│   │   ├── User.js
│   │   ├── Product.js
│   │   ├── Bid.js
│   │   ├── Deal.js
│   │   ├── Chat.js
│   │   └── Message.js
│   │
│   ├── routes
│   │   ├── user.routes.js
│   │   ├── product.routes.js
│   │   ├── bid.routes.js
│   │   ├── deal.routes.js
│   │   └── chat.routes.js
│   │
│   ├── middleware
│   │   ├── auth.middleware.js
│   │   ├── error.middleware.js
│   │   └── validate.middleware.js
│   │
│   ├── validators
│   │   ├── user.schema.js
│   │   ├── product.schema.js
│   │   └── bid.schema.js
│   │
│   ├── utils
│   │
│   ├── app.js
│   └── server.js
│
├── tests
│
├── .env
├── package.json
└── README.md
```

---

# 8. Authentication Requirements

## 8.1 Authentication Flow

```text
Android App
      │
      ▼
Firebase Authentication
      │
      ▼
Firebase ID Token
      │
      ▼
HTTP Authorization Header
      │
      ▼
Node.js API
      │
      ▼
Firebase Admin SDK
      │
      ▼
Token Validation
      │
      ▼
Authorized Request
```

---

## 8.2 API Authorization Header

```http
Authorization: Bearer <FIREBASE_ID_TOKEN>
```

All protected API endpoints must verify the Firebase ID token.

---

# 9. User Roles

FarmBid will support two roles.

```text
FARMER
BUYER
```

Role-based authorization must be implemented on the backend.

---

## 9.1 Farmer Permissions

| Feature             | Access |
| ------------------- | ------ |
| Browse products     | Yes    |
| Create products     | Yes    |
| Update own products | Yes    |
| Delete own products | Yes    |
| View received bids  | Yes    |
| Accept bid          | Yes    |
| Reject bid          | Yes    |
| Create bid          | No     |
| Access deal chat    | Yes    |

---

## 9.2 Buyer Permissions

| Feature          | Access |
| ---------------- | ------ |
| Browse products  | Yes    |
| View products    | Yes    |
| Place bid        | Yes    |
| Update own bid   | Yes    |
| Cancel own bid   | Yes    |
| Create products  | No     |
| Accept bids      | No     |
| Access deal chat | Yes    |

---

# 10. Database Requirements

FarmBid will use MongoDB.

Collections:

```text
users
products
bids
deals
chats
messages
```

MongoDB ObjectIds will define relationships between documents.

---

# 11. User Schema

```text
User
├── _id
├── firebaseUid
├── role
├── fullName
├── email
├── phoneNumber
├── profileImage
├── language
├── location
│   ├── address
│   ├── latitude
│   └── longitude
├── isProfileComplete
├── createdAt
└── updatedAt
```

## Required Constraints

- `firebaseUid` must be unique.
- `email` must be unique when present.
- `role` must be `FARMER` or `BUYER`.

---

# 12. Product Schema

```text
Product
├── _id
├── farmerId
├── name
├── quantity
├── unit
├── basePrice
├── location
│   ├── address
│   ├── latitude
│   └── longitude
├── harvestDate
├── quality
├── images
├── status
├── createdAt
└── updatedAt
```

## Product Constraints

- `farmerId` is required.
- Product must contain 2 to 4 images.
- Quantity must be greater than zero.
- Harvest date cannot be in the future.
- Only farmers can create products.

---

# 13. Bid Schema

```text
Bid
├── _id
├── productId
├── buyerId
├── farmerId
├── offeredPrice
├── requestedQuantity
├── status
├── createdAt
└── updatedAt
```

## Bid Status

```text
PENDING
ACCEPTED
REJECTED
CANCELLED
EXPIRED
```

## Bid Constraints

- Buyer cannot bid on their own product.
- Product must be available.
- Requested quantity must be greater than zero.
- Offered price must be greater than zero.
- A bid cannot be modified after acceptance.
- Only one bid can be accepted for a product listing.

---

# 14. Deal Schema

```text
Deal
├── _id
├── productId
├── farmerId
├── buyerId
├── acceptedBidId
├── finalPrice
├── quantity
├── status
├── createdAt
└── completedAt
```

## Deal Status

```text
ACTIVE
COMPLETED
CANCELLED
```

---

# 15. Chat Schema

```text
Chat
├── _id
├── farmerId
├── buyerId
├── dealId
├── lastMessageAt
└── createdAt
```

A chat should be created only after a deal is created.

---

# 16. Message Schema

```text
Message
├── _id
├── chatId
├── senderId
├── content
├── messageType
├── isRead
└── createdAt
```

## Supported Message Types

```text
TEXT
```

The MVP supports text messages only.

---

# 17. API Requirements

## 17.1 User APIs

```http
POST /api/users/profile
GET /api/users/me
PUT /api/users/me
GET /api/users/:id
```

---

## 17.2 Product APIs

```http
POST /api/products
GET /api/products
GET /api/products/:id
PUT /api/products/:id
DELETE /api/products/:id
GET /api/products/my-products
```

---

## 17.3 Bid APIs

```http
POST /api/products/:productId/bids
GET /api/products/:productId/bids
GET /api/bids/my-bids
PUT /api/bids/:id
POST /api/bids/:id/cancel
POST /api/bids/:id/accept
POST /api/bids/:id/reject
```

---

## 17.4 Deal APIs

```http
GET /api/deals
GET /api/deals/:id
POST /api/deals/:id/complete
POST /api/deals/:id/cancel
```

---

## 17.5 Chat APIs

```http
GET /api/chats
GET /api/chats/:id/messages
POST /api/chats/:id/messages
POST /api/chats/:id/read
```

---

# 18. API Response Format

All APIs should follow a consistent response structure.

## Success Response

```json
{
  "success": true,
  "message": "Product created successfully",
  "data": {}
}
```

## Error Response

```json
{
  "success": false,
  "message": "Validation failed",
  "errors": []
}
```

---

# 19. HTTP Status Codes

| Status Code | Usage                 |
| ----------- | --------------------- |
| 200         | Successful request    |
| 201         | Resource created      |
| 400         | Invalid request       |
| 401         | Unauthenticated       |
| 403         | Unauthorized          |
| 404         | Resource not found    |
| 409         | Conflict              |
| 500         | Internal server error |

---

# 20. Image Upload Requirements

Product images must be stored in Cloudinary.

Requirements:

- Minimum 2 images.
- Maximum 4 images.
- Supported formats: JPEG, JPG, PNG, WebP.
- Images should be compressed before upload where practical.
- Cloudinary secure URLs must be stored in MongoDB.

Recommended flow:

```text
Android App
      ↓
Select Images
      ↓
Upload to Backend
      ↓
Backend Uploads to Cloudinary
      ↓
Cloudinary Returns URLs
      ↓
Backend Creates Product
      ↓
MongoDB Stores URLs
```

---

# 21. Product Search and Filtering

The marketplace should support:

- Product name search.
- Location filter.
- Price sorting.
- Newest products.
- Available quantity.

Recommended query format:

```http
GET /api/products?search=wheat&location=jamshedpur
```

---

# 22. Chat Refresh Requirements

The MVP will not use WebSockets.

The Android application will refresh messages using polling or manual refresh.

Recommended approach:

```text
Open Chat
    ↓
Fetch Messages
    ↓
Display Messages
    ↓
Wait 5–10 Seconds
    ↓
Fetch Latest Messages
```

Polling should stop when the chat screen is not visible.

---

# 23. Location Requirements

Location is used for:

- User profile.
- Product listing.
- Product discovery.

The application may request device location permission.

Live tracking is not required.

If location permission is denied, the user should be able to enter location manually.

---

# 24. Localization Requirements

FarmBid must support:

```text
English
Hindi
```

Android resource structure:

```text
res/
├── values/
│   └── strings.xml
│
└── values-hi/
    └── strings.xml
```

Language preference should be stored locally using DataStore and synchronized with the user profile.

---

# 25. Security Requirements

## Authentication

- Firebase ID tokens must be validated by the backend.
- Protected APIs must reject missing or invalid tokens.

## Authorization

- Farmers can modify only their own products.
- Buyers can modify only their own bids.
- Only the product owner can accept or reject bids.
- Users can access only their own chats and deals.

## Input Validation

All backend input must be validated.

Validation must cover:

- Required fields.
- String lengths.
- Numeric values.
- Object IDs.
- Dates.
- Image counts.
- Allowed statuses.

---

# 26. Environment Variables

The backend must not commit secrets to Git.

Example `.env` variables:

```text
PORT=5000
NODE_ENV=development

MONGODB_URI=

FIREBASE_PROJECT_ID=
FIREBASE_CLIENT_EMAIL=
FIREBASE_PRIVATE_KEY=

CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=
```

The `.env` file must be included in `.gitignore`.

---

# 27. Render Deployment Requirements

The backend will be deployed to Render.

Required configuration:

- Node.js environment.
- Production environment variables.
- MongoDB Atlas connection.
- Firebase Admin credentials.
- Cloudinary credentials.
- HTTPS enabled.

The backend must provide a health endpoint:

```http
GET /health
```

Example response:

```json
{
  "success": true,
  "message": "FarmBid API is running"
}
```

---

# 28. Error Handling

The backend must have centralized error handling.

Requirements:

- No raw server stack traces in production responses.
- Consistent API error structure.
- Validation errors should be understandable.
- Database errors should be handled safely.

Recommended middleware:

```text
error.middleware.js
```

---

# 29. Testing Requirements

Testing must happen after each stable feature.

## Android Testing

Verify:

- Application startup.
- Authentication.
- Navigation.
- Form validation.
- API communication.
- Error handling.
- Product listing.
- Image selection.
- Bidding.
- Deals.
- Chat.
- Language switching.

---

## Backend Testing

Verify:

- Authentication middleware.
- User APIs.
- Product APIs.
- Bid APIs.
- Deal APIs.
- Chat APIs.
- Authorization.
- Validation.
- Database operations.

---

# 30. Mandatory Build Verification

After every stable Android phase:

```bash
./gradlew build
```

The project must build successfully before a stable phase is committed.

Before a major release:

```bash
./gradlew clean
./gradlew build
```

---

# 31. Development Workflow

Every feature must follow:

```text
PLAN
  ↓
IMPLEMENT
  ↓
RUN APPLICATION
  ↓
TEST FEATURE
  ↓
FIX ERRORS
  ↓
TEST AGAIN
  ↓
RUN GRADLE BUILD
  ↓
VERIFY BACKEND
  ↓
GIT COMMIT
```

---

# 32. Development Phases

## Phase 1: Foundation

- Create Android project.
- Configure package name.
- Create backend.
- Create MongoDB database.
- Create Git repository.
- Configure environment variables.
- Configure Render.

**Verification:**

```bash
./gradlew build
```

**Commit:**

```text
chore: initialize FarmBid Android and backend projects
```

---

## Phase 2: Authentication

- Firebase setup.
- Email/password authentication.
- Google Sign-In.
- Forgot password.
- Logout.
- Firebase token validation.

**Commit:**

```text
feat(auth): implement Firebase authentication and Google sign-in
```

---

## Phase 3: User Profile

- Role selection.
- Profile creation.
- Profile editing.
- Location support.
- Language preferences.

**Commit:**

```text
feat(profile): implement user profile and role management
```

---

## Phase 4: Product Management

- Product model.
- Product APIs.
- Add product screen.
- Edit product.
- Delete product.
- Cloudinary upload.
- Inventory management.

**Commit:**

```text
feat(products): implement product listing and inventory management
```

---

## Phase 5: Marketplace

- Product browsing.
- Product details.
- Search.
- Basic filtering.

**Commit:**

```text
feat(marketplace): add product discovery and search
```

---

## Phase 6: Bidding

- Bid creation.
- Bid listing.
- Farmer bid management.
- Accept bid.
- Reject bid.
- Cancel bid.

**Commit:**

```text
feat(bidding): implement competitive product bidding workflow
```

---

## Phase 7: Deals

- Deal creation.
- Active deals.
- Completed deals.
- Deal status management.

**Commit:**

```text
feat(deals): implement accepted bid deal management
```

---

## Phase 8: Chat

- Chat creation.
- Send messages.
- Fetch messages.
- Polling.
- Message read status.

**Commit:**

```text
feat(chat): add polling-based deal messaging
```

---

## Phase 9: Localization

- English resources.
- Hindi resources.
- Language switching.
- DataStore persistence.

**Commit:**

```text
feat(localization): add English and Hindi language support
```

---

## Phase 10: Final Testing

- Full authentication flow.
- Full farmer flow.
- Full buyer flow.
- Product listing.
- Image upload.
- Bidding.
- Deal creation.
- Chat.
- Backend deployment.
- Gradle build.

**Commit:**

```text
test: complete end-to-end MVP verification
```

---

# 33. Git Workflow

Every stable phase must be committed.

Example:

```bash
git status
git add .
git commit -m "feat(products): implement product listing"
git push origin main
```

Commit messages must follow a professional convention.

Recommended prefixes:

```text
feat
fix
refactor
test
docs
chore
```

---

# 34. Build Quality Requirements

The following must be verified before each stable commit:

```text
✓ Android app launches
✓ No critical crashes
✓ Backend server starts
✓ Database connection works
✓ APIs respond correctly
✓ Firebase authentication works
✓ Cloudinary upload works
✓ Feature works as expected
✓ Gradle build succeeds
✓ Changes are committed to GitHub
```

---

# 35. Future Technical Improvements

The following are outside the MVP but can be added later:

- WebSocket chat.
- Firebase Cloud Messaging.
- Push notifications.
- Payment gateway.
- UPI.
- Transport integration.
- Advanced search.
- Elasticsearch.
- Product recommendations.
- AI price recommendations.
- Farmer verification.
- Ratings and reviews.
- Admin dashboard.
- Web platform.

---

# 36. Definition of Done

A feature is complete only when:

1. The feature is implemented.
2. The feature works on the Android application.
3. The backend implementation works.
4. Database operations are correct.
5. API errors are handled.
6. Role-based authorization is tested.
7. The application does not have known critical errors.
8. `./gradlew build` succeeds.
9. The backend is tested.
10. The stable implementation is committed to GitHub.

---

# 37. Final Technical Summary

FarmBid will use:

```text
ANDROID
├── Kotlin
├── Jetpack Compose
├── MVVM
├── Retrofit
├── Hilt
├── Coroutines
├── Firebase Auth
└── Coil

BACKEND
├── Node.js
├── Express.js
├── REST API
├── Firebase Admin SDK
├── Mongoose
└── Zod

DATABASE
└── MongoDB

CLOUD SERVICES
├── Firebase
├── Cloudinary
├── MongoDB Atlas
└── Render

VERSION CONTROL
└── GitHub
```

The final technical workflow is:

```text
ANDROID APP
      │
      │ HTTPS
      ▼
NODE.JS + EXPRESS
      │
      ├── Firebase Authentication
      │
      ├── MongoDB
      │
      └── Cloudinary
```

**FarmBid will be developed as a stable, phase-based Android marketplace application with the bidding system as its primary technical and product feature.**
