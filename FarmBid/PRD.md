# FarmBid – Product Requirements Document (PRD)

**Project Name:** FarmBid **Package Name:** `com.buildstack.farmbid` **Platform:** Android **Application Type:** Agricultural B2B Marketplace **Primary Language:** English and Hindi **Document Version:** 1.0 **Status:** MVP / College Project

---

# 1. Product Overview

FarmBid is an Android-based agricultural marketplace that connects farmers directly with buyers who want to purchase agricultural products in bulk.

The core mechanic of FarmBid is a **bidding system**.

Farmers can list their harvested agricultural products, including quantity, price, location, images, harvest date, and quality information. Buyers can browse available products and place bids. Multiple buyers can bid on the same product listing, and the farmer can select the most suitable bid.

Once a bid is accepted, a deal is created between the farmer and buyer. They can communicate through the application's chat system and share contact information to complete the transaction outside the platform.

FarmBid does not process payments, manage transportation, or perform product quality verification.

---

# 2. Problem Statement

Farmers may have difficulty finding suitable buyers directly for their agricultural products. At the same time, buyers may not have a simple way to discover available products directly from farmers.

FarmBid solves this by providing a marketplace where:

- Farmers can list harvested products.
- Buyers can browse available products.
- Buyers can compete by placing bids.
- Farmers can select the best buyer.
- Both parties can communicate directly after a deal is created.

The platform acts as a connection layer between farmers and buyers.

---

# 3. Target Users

FarmBid has two user roles.

## 3.1 Farmer

A farmer can:

- Create and manage a profile.
- Add agricultural products.
- Upload product images.
- Manage available inventory.
- Set a base price.
- View bids from multiple buyers.
- Accept or reject bids.
- Create a deal by accepting a bid.
- Communicate with buyers.
- Update product inventory.
- Mark a deal as completed.

---

## 3.2 Buyer

A buyer can:

- Create and manage a profile.
- Browse agricultural products.
- Search available products.
- View product details.
- Check available quantity.
- View farmer information.
- Place bids.
- Update or manage bids before acceptance.
- View accepted or rejected bids.
- Communicate with farmers after a deal is created.
- View their deals.

The initial product focus is on buyers purchasing agricultural products in bulk.

---

# 4. Core Product Concept

The primary FarmBid workflow is:

```text
Farmer lists product
        ↓
Product becomes available in marketplace
        ↓
Multiple buyers view product
        ↓
Buyers place bids
        ↓
Farmer reviews bids
        ↓
Farmer accepts one suitable bid
        ↓
Deal is created
        ↓
Farmer and buyer communicate
        ↓
Transaction happens outside the application
        ↓
Deal is marked as completed
```

---

# 5. Product Goals

## 5.1 Primary Goals

- Build a simple agricultural marketplace.
- Connect farmers directly with buyers.
- Allow farmers to sell products in bulk.
- Implement a competitive bidding system.
- Allow users to communicate after a deal is created.
- Support English and Hindi.
- Create a stable Android application with a scalable backend.

---

## 5.2 Non-Goals for MVP

The following features are intentionally excluded from the initial version:

- In-app payments.
- Payment gateway integration.
- Transportation services.
- Live vehicle tracking.
- Live product market prices.
- Product quality verification.
- Ratings and reviews.
- Advanced farmer verification.
- Real-time chat using WebSockets.
- Live location tracking.
- AI price recommendations.

These features may be considered in future versions.

---

# 6. Authentication

FarmBid will provide the following authentication features.

## 6.1 Sign Up

Users can create an account using:

- Email and password.
- Google Sign-In.

After registration, the user must select a role:

- Farmer
- Buyer

---

## 6.2 Login

Users can log in using:

- Email and password.
- Google authentication.

---

## 6.3 Forgot Password

Users can reset their password through the configured authentication service.

---

## 6.4 Logout

Users can securely log out from their account.

---

## 6.5 Authentication Requirements

The application will use Firebase Authentication for:

- Email and password authentication.
- Google Sign-In.
- Authentication state management.
- Password reset functionality.

The Node.js backend will validate the Firebase authentication token before allowing access to protected APIs.

---

# 7. User Profile

Every user will have a profile.

## 7.1 Common Profile Information

A user profile may include:

- Full name.
- Email address.
- Phone number.
- Profile image.
- User role.
- Preferred language.
- Address.
- Location.
- Latitude.
- Longitude.

The user's location can be obtained from device GPS when required.

FarmBid will not track live location.

---

## 7.2 Farmer Profile

The farmer profile may additionally display:

- Available products.
- Inventory.
- Active listings.
- Active deals.
- Completed deals.

---

## 7.3 Buyer Profile

The buyer profile may additionally display:

- Placed bids.
- Accepted bids.
- Active deals.
- Completed deals.

---

# 8. Product Listing

Only farmers can create product listings.

A farmer can list only products that have already been harvested.

Future or unharvested products cannot be listed.

---

## 8.1 Product Listing Fields

Each product listing must contain:

- Product name.
- Available quantity.
- Quantity unit.
- Base price.
- Location.
- 2 to 4 product images.
- Harvest date.
- Quality description.
- Listing status.

---

## 8.2 Example Product

```text
Product Name: Wheat
Quantity: 20 KG
Base Price: ₹30 per KG
Location: Jamshedpur
Harvest Date: 20 August 2026
Quality: Grade A
Images: 3
Status: Available
```

---

## 8.3 Product Images

Each product must have:

- Minimum: 2 images.
- Maximum: 4 images.

Images will be stored using Cloudinary.

The database will store the returned secure image URLs.

---

# 9. Inventory Management

Farmers can manage their inventory.

Example:

```text
Wheat: 20 KG
Rice: 40 KG
Potato: 100 KG
```

The farmer can:

- Add inventory.
- Update quantity.
- Update product information.
- Remove a product.
- Mark a product as unavailable.

Buyers can view the currently available quantity.

---

# 10. Marketplace

The marketplace will display products listed by farmers.

Buyers can:

- Browse products.
- View product details.
- Search for products.
- View product images.
- View available quantity.
- View location.
- View harvest date.
- View quality information.
- View the farmer's information.
- Place a bid.

---

# 11. Bidding System

The bidding system is the primary feature of FarmBid.

A buyer can place a bid on an available product.

Multiple buyers can place bids on the same product.

---

## 11.1 Bid Information

A bid contains:

- Product ID.
- Buyer ID.
- Farmer ID.
- Offered price.
- Requested quantity.
- Bid status.
- Creation date.

---

## 11.2 Bid Status

A bid can have the following statuses:

```text
PENDING
ACCEPTED
REJECTED
CANCELLED
EXPIRED
```

---

## 11.3 Farmer Bid Management

A farmer can:

- View all bids for a product.
- Compare bid prices.
- Compare requested quantities.
- Accept one bid.
- Reject bids.
- Select the most suitable buyer.

The farmer is not required to accept the highest bid. The farmer can choose the bid they consider most suitable.

---

# 12. Bid Acceptance and Deal Creation

When a farmer accepts a bid:

1. The selected bid status becomes `ACCEPTED`.
2. A deal is created.
3. Other competing bids are no longer eligible for acceptance.
4. The buyer is notified through the application.
5. The farmer and buyer can access the deal.
6. Communication becomes available between both users.

A deal represents the agreement between the farmer and buyer.

---

# 13. Deal Management

A deal contains:

- Product ID.
- Farmer ID.
- Buyer ID.
- Accepted bid ID.
- Final agreed price.
- Agreed quantity.
- Deal status.
- Creation date.
- Completion date.

---

## 13.1 Deal Status

A deal can have the following statuses:

```text
ACTIVE
COMPLETED
CANCELLED
```

---

## 13.2 Transaction Handling

FarmBid does not process payments.

The farmer and buyer are responsible for completing:

- Payment.
- Product collection.
- Delivery.
- Transportation.

These activities happen outside the FarmBid platform.

---

# 14. Chat System

FarmBid will provide a simple chat system.

The chat is not real-time in the initial version.

Messages will be refreshed or polled to retrieve the latest conversation.

---

## 14.1 Chat Features

Users can:

- Send text messages.
- View received messages.
- Refresh for new messages.
- View conversation history.

---

## 14.2 Chat Restrictions

Chat access should be associated with an active or completed deal.

This prevents unnecessary messaging between unrelated users.

---

## 14.3 Contact Information

After a deal is created, the farmer and buyer can access each other's relevant contact information.

This allows them to continue communication outside the application when required.

---

# 15. Language Support

FarmBid will support:

- English.
- Hindi.

The application should use Android localization resources.

Example:

```text
res/values/strings.xml
res/values-hi/strings.xml
```

Users can change the application language from settings.

---

# 16. Application Screens

The initial MVP should contain the following screens.

## Authentication

- Splash screen.
- Login screen.
- Sign-up screen.
- Google Sign-In.
- Forgot password screen.
- Role selection screen.

## Profile

- Profile setup screen.
- Edit profile screen.
- Profile overview.
- Settings.
- Language selection.

## Farmer

- Farmer home.
- My products.
- Add product.
- Edit product.
- Product details.
- Inventory management.
- Received bids.
- Bid details.
- Active deals.
- Completed deals.

## Buyer

- Buyer home.
- Product marketplace.
- Product search.
- Product details.
- Place bid.
- My bids.
- Bid status.
- Active deals.
- Completed deals.

## Communication

- Chat list.
- Chat conversation.
- Deal details.
- Contact information.

---

# 17. Technical Architecture

## 17.1 Android Application

**Technology:**

- Kotlin.
- Android SDK.
- MVVM architecture.
- Jetpack components.
- Retrofit for API communication.
- Coroutines for asynchronous operations.
- Firebase Authentication.

---

## 17.2 Backend

**Technology:**

- Node.js.
- Express.js.
- REST API architecture.

The backend will handle:

- User management.
- Product management.
- Inventory management.
- Bidding.
- Deal creation.
- Chat.
- Message storage.
- Firebase token validation.

---

## 17.3 Database

**Technology:**

- MongoDB.

Main collections:

```text
Users
Products
Bids
Deals
Chats
Messages
```

---

## 17.4 Image Storage

**Technology:**

- Cloudinary.

Cloudinary will store:

- Product images.
- Optional profile images.

MongoDB will store the secure image URLs.

---

## 17.5 Authentication

**Technology:**

- Firebase Authentication.
- Google Sign-In.
- Firebase ID tokens.

---

## 17.6 Deployment

**Backend Deployment:**

- Render.

The backend environment will contain secure environment variables for:

- MongoDB connection.
- Firebase credentials.
- Cloudinary credentials.
- JWT or token configuration if required.
- Application configuration.

---

# 18. API Architecture

The backend will expose REST APIs.

Example API groups:

```text
/api/auth
/api/users
/api/products
/api/bids
/api/deals
/api/chats
/api/messages
```

---

## 18.1 Protected API Access

The API flow will be:

```text
Android Application
        ↓
Firebase Authentication
        ↓
Firebase ID Token
        ↓
Node.js Backend
        ↓
Firebase Token Validation Middleware
        ↓
Protected API Access
```

---

# 19. High-Level System Flow

```text
Android Kotlin App
        |
        | HTTPS REST API
        v
Node.js + Express Backend
        |
        |---------------- Firebase Authentication
        |
        |---------------- MongoDB
        |                    |
        |                    |-- Users
        |                    |-- Products
        |                    |-- Bids
        |                    |-- Deals
        |                    |-- Chats
        |                    |-- Messages
        |
        |---------------- Cloudinary
        |                    |
        |                    |-- Product Images
        |
        |---------------- Render
                             |
                             |-- Backend Deployment
```

---

# 20. Data Models

## 20.1 User

```text
_id
firebaseUid
role
fullName
email
phoneNumber
profileImage
language
locationAddress
latitude
longitude
isProfileComplete
createdAt
updatedAt
```

---

## 20.2 Product

```text
_id
farmerId
name
quantity
unit
basePrice
location
latitude
longitude
harvestDate
quality
images
status
createdAt
updatedAt
```

---

## 20.3 Bid

```text
_id
productId
buyerId
farmerId
offeredPrice
requestedQuantity
status
createdAt
updatedAt
```

---

## 20.4 Deal

```text
_id
productId
farmerId
buyerId
acceptedBidId
finalPrice
quantity
status
createdAt
completedAt
```

---

## 20.5 Chat

```text
_id
farmerId
buyerId
dealId
lastMessageAt
createdAt
```

---

## 20.6 Message

```text
_id
chatId
senderId
content
messageType
isRead
createdAt
```

---

# 21. Product Lifecycle

A product can follow this lifecycle:

```text
DRAFT
    ↓
AVAILABLE
    ↓
BIDS RECEIVED
    ↓
BID ACCEPTED
    ↓
DEAL CREATED
    ↓
SOLD
```

---

# 22. Development Workflow

The project must be developed in phases.

Every phase follows the same loop:

```text
PLAN
  ↓
BUILD
  ↓
TEST
  ↓
FIX
  ↓
VERIFY
  ↓
GRADLE BUILD
  ↓
GIT COMMIT
```

---

# 23. Mandatory Development Rules

## Rule 1: Phase-Wise Development

Do not build the entire project at once.

Complete one stable phase before moving to the next.

---

## Rule 2: Build-Test-Fix Loop

For every feature:

```text
Build
→ Test
→ Identify Problems
→ Fix Problems
→ Test Again
→ Verify Stability
```

---

## Rule 3: Mandatory Gradle Verification

After each stable Android implementation:

```bash
./gradlew build
```

The build must be checked for errors before the phase is considered complete.

---

## Rule 4: Backend Testing

Backend APIs must be tested before Android integration.

Testing should verify:

- Request validation.
- Authentication.
- Authorization.
- Successful responses.
- Error responses.
- Database operations.

---

## Rule 5: Full Integration Testing

Before every major commit, test:

- Android frontend.
- Node.js backend.
- MongoDB operations.
- Firebase authentication.
- Cloudinary image upload.
- API communication.

---

## Rule 6: Professional Git Commits

Every stable phase should be committed to GitHub.

Example commit messages:

```text
feat(auth): implement Firebase email authentication

feat(profile): add farmer and buyer profile setup

feat(products): implement product listing and inventory management

feat(bidding): add product bidding system

feat(deals): implement bid acceptance and deal creation

feat(chat): add polling-based messaging system

fix(api): resolve product authorization issue

fix(android): handle network error in product listing
```

---

# 24. Development Phases

## Phase 1: Project Setup

- Create Android project.
- Configure package name.
- Create GitHub repository.
- Configure Node.js backend.
- Configure MongoDB.
- Configure Render deployment.
- Set up environment variables.
- Run initial Gradle build.

---

## Phase 2: Authentication

- Firebase setup.
- Email/password registration.
- Login.
- Google Sign-In.
- Forgot password.
- Logout.
- Token validation.

---

## Phase 3: Profile and Role System

- Role selection.
- Farmer profile.
- Buyer profile.
- Profile editing.
- Location integration.
- Language selection.

---

## Phase 4: Product and Inventory Management

- Product creation.
- Product editing.
- Product deletion.
- Inventory management.
- Image upload.
- Cloudinary integration.
- Marketplace display.

---

## Phase 5: Bidding System

- Place bid.
- View bids.
- Bid status.
- Farmer bid management.
- Accept bid.
- Reject bid.
- Prevent invalid bid acceptance.

---

## Phase 6: Deal System

- Create deal from accepted bid.
- Active deals.
- Completed deals.
- Deal status management.

---

## Phase 7: Chat

- Create chat.
- Send message.
- Retrieve messages.
- Polling or refresh.
- Chat history.

---

## Phase 8: Localization

- English language support.
- Hindi language support.
- Language switching.
- UI translation.

---

## Phase 9: Testing

- Unit testing.
- API testing.
- Integration testing.
- Authentication testing.
- Product flow testing.
- Bid flow testing.
- Deal flow testing.
- Chat testing.
- Full user flow testing.

---

## Phase 10: Final Verification

Before project completion:

```text
Test Backend
    ↓
Test Database
    ↓
Test Firebase
    ↓
Test Cloudinary
    ↓
Test Android Application
    ↓
Run Gradle Build
    ↓
Fix All Errors
    ↓
Final Testing
    ↓
Professional Git Commit
```

---

# 25. MVP Success Criteria

The FarmBid MVP is considered complete when:

- Users can register and log in.
- Users can choose Farmer or Buyer role.
- Users can create and update profiles.
- Farmers can add harvested products.
- Farmers can upload 2 to 4 images.
- Farmers can manage inventory.
- Buyers can browse products.
- Buyers can place bids.
- Farmers can view multiple bids.
- Farmers can accept one bid.
- A deal is created after bid acceptance.
- Farmer and buyer can communicate.
- Messages can be refreshed.
- The application supports English and Hindi.
- All backend APIs work correctly.
- Product images upload successfully.
- Authentication works correctly.
- The Android application passes Gradle build.
- Stable project phases are committed to GitHub.

---

# 26. Future Enhancements

Potential future versions may include:

- Online payments.
- UPI integration.
- Transporter marketplace.
- Live market prices.
- Farmer verification.
- Ratings and reviews.
- Product quality certificates.
- Real-time chat.
- Push notifications.
- AI price suggestions.
- Advanced search and filters.
- Order tracking.
- Delivery tracking.
- Web application.

---

# 27. Final Product Vision

FarmBid aims to become a simple digital bridge between agricultural producers and bulk buyers.

The core value proposition is:

```text
LIST
    ↓
BID
    ↓
SELECT
    ↓
CONNECT
    ↓
DEAL
```

Farmers list harvested products.

Buyers compete by placing bids.

Farmers choose the most suitable buyer.

Both users connect and complete the transaction.

**FarmBid – A Simple Marketplace for Farmers and Buyers.**
