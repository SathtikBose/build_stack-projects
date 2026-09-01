# FarmBid – Project Development Instructions

**Project Name:** FarmBid **Package Name:** `com.buildstack.farmbid` **Platform:** Android **Frontend:** Kotlin + Jetpack Compose **Backend:** Node.js + Express.js **Database:** MongoDB **Authentication:** Firebase Authentication **Image Storage:** Cloudinary **UI/UX Design Tool:** Google Stitch **Deployment:** Render **Document Version:** 1.0

---

# 1. Purpose

This document defines the mandatory rules and instructions for building FarmBid.

The development process must follow a structured loop:

```text
PLAN
  ↓
BUILD
  ↓
TEST
  ↓
FIX
  ↓
BUILD AGAIN
  ↓
VERIFY
  ↓
COMMIT TO GITHUB
```

No major feature should be considered complete without testing.

---

# 2. Primary Development Rule

## Build Phase by Phase

Do not build the entire application at once.

The project must be divided into stable phases.

Each phase must follow:

```text
UNDERSTAND REQUIREMENTS
        ↓
PLAN THE FEATURE
        ↓
DESIGN THE UI
        ↓
BUILD
        ↓
TEST
        ↓
FIX
        ↓
VERIFY
        ↓
GRADLE BUILD / BACKEND TEST
        ↓
GIT COMMIT
```

---

# 3. Mandatory UI/UX Design Rule

## Google Stitch Is Mandatory

All major UI screens must be designed using **Google Stitch** before implementation.

Required workflow:

```text
FEATURE REQUIREMENT
        ↓
READ UI/UX BRIEF
        ↓
CREATE STITCH PROMPT
        ↓
GENERATE UI IN GOOGLE STITCH
        ↓
REVIEW DESIGN
        ↓
REFINE DESIGN
        ↓
APPROVE DESIGN
        ↓
IMPLEMENT IN JETPACK COMPOSE
```

Do not randomly design major application screens directly in code without first following the design process.

---

# 4. Source of Truth

The following documents define the project requirements:

```text
docs/
├── prd.md
├── trd.md
├── database-schema.md
├── endpoints.md
├── ui-ux-brief.md
├── instruction.md
└── build-flow.md
```

Before building a feature:

1. Read the relevant requirements.
2. Check the technical design.
3. Check the database requirements.
4. Check the API requirements.
5. Check the UI/UX requirements.

Do not introduce new features without considering their impact on the existing system.

---

# 5. Tech Stack Rules

## Android Application

Use:

```text
Kotlin
Jetpack Compose
MVVM Architecture
Retrofit
Firebase Authentication
Google Sign-In
Coroutines
Flow / StateFlow
```

Recommended additional libraries:

```text
Coil
Hilt
Navigation Compose
```

---

## Backend

Use:

```text
Node.js
Express.js
MongoDB
Mongoose
Firebase Admin SDK
Cloudinary
```

---

# 6. Project Architecture

## Android

Recommended structure:

```text
com.buildstack.farmbid
│
├── data
│   ├── remote
│   ├── repository
│   └── local
│
├── domain
│   ├── model
│   └── usecase
│
├── ui
│   ├── auth
│   ├── profile
│   ├── home
│   ├── product
│   ├── bid
│   ├── deal
│   ├── chat
│   └── theme
│
├── navigation
│
├── di
│
└── MainActivity.kt
```

Do not place all application logic inside Activities or Composables.

---

## Backend

Recommended structure:

```text
backend/
│
├── src/
│   ├── config/
│   ├── controllers/
│   ├── services/
│   ├── models/
│   ├── routes/
│   ├── middleware/
│   ├── validators/
│   └── utils/
│
├── server.js
├── package.json
└── .env.example
```

The architecture should follow:

```text
ROUTE
  ↓
MIDDLEWARE
  ↓
CONTROLLER
  ↓
SERVICE
  ↓
MODEL
  ↓
DATABASE
```

---

# 7. Authentication Rules

FarmBid authentication uses Firebase.

Supported features:

- Email and password registration.
- Email and password login.
- Google Sign-In.
- Forgot password.
- Logout.

The backend must not handle raw passwords.

The backend should receive:

```text
Firebase ID Token
```

The backend must:

1. Verify the Firebase token.
2. Extract the Firebase UID.
3. Find the FarmBid user profile.
4. Attach the authenticated user to the request.

---

# 8. User Role Rules

FarmBid has exactly two roles:

```text
FARMER
BUYER
```

Do not add:

```text
ADMIN
TRANSPORTER
WHOLESALER
SUPPLIER
```

A wholesaler uses the `BUYER` role in the MVP.

Role permissions must be enforced by the backend.

---

# 9. Product Rules

Only farmers can create products.

Every product must include:

- Product name.
- Quantity.
- Unit.
- Base price.
- Location.
- 2 to 4 images.
- Harvest date.
- Quality.

Mandatory validation:

```text
Quantity > 0
Base Price > 0
Images >= 2
Images <= 4
Harvest Date <= Current Date
```

The system must not allow farmers to list products that have not yet been harvested.

---

# 10. Product Image Rules

Product images must:

1. Be uploaded through the Android application.
2. Be sent to the backend.
3. Be uploaded to Cloudinary.
4. Store the Cloudinary secure URL and public ID in MongoDB.

The Android application must not store permanent image URLs as hardcoded local data.

---

# 11. Bidding Rules

The bid system is the core FarmBid mechanic.

Only buyers can place bids.

A buyer provides:

- Offered price.
- Requested quantity.

Rules:

```text
Buyer cannot bid on own product.
Product must be AVAILABLE.
Bid must be greater than zero.
Requested quantity must be greater than zero.
Requested quantity cannot exceed available quantity.
```

A farmer can:

```text
ACCEPT
REJECT
```

any pending bid.

The farmer is not required to accept the highest bid.

---

# 12. Bid Acceptance Rules

When a farmer accepts a bid:

```text
ACCEPT BID
    ↓
Validate Farmer Ownership
    ↓
Validate Bid Status
    ↓
Validate Product Availability
    ↓
Accept Selected Bid
    ↓
Reject Other Pending Bids
    ↓
Create Deal
    ↓
Create Chat
    ↓
Update Product Status
```

This operation must be protected against duplicate acceptance.

Use MongoDB transactions or another safe atomic approach.

---

# 13. Deal Rules

A deal is created after accepting a bid.

The deal includes:

- Product.
- Farmer.
- Buyer.
- Accepted bid.
- Final price.
- Quantity.
- Deal status.

Supported statuses:

```text
ACTIVE
COMPLETED
CANCELLED
```

There is no payment processing.

There is no escrow.

There is no payment verification.

---

# 14. Chat Rules

The chat is available only after a deal is created.

Chat participants:

```text
Farmer
Buyer
```

The MVP does not require real-time messaging.

The application uses:

```text
REST API
+
POLLING
```

Recommended polling interval:

```text
5 to 10 seconds
```

Polling must stop when the chat screen is closed.

---

# 15. Location Rules

The application uses location for displaying product and user location.

Location can be obtained through:

```text
GPS
OR
MANUAL INPUT
```

FarmBid does not require live location tracking.

Do not continuously track the user's location.

---

# 16. Language Rules

The application supports:

```text
English
हिन्दी
```

Do not hardcode user-facing strings inside Compose UI.

Use Android string resources.

Recommended structure:

```text
values/
    strings.xml

values-hi/
    strings.xml
```

---

# 17. API Development Rules

Every API must include:

```text
Authentication
Authorization
Validation
Error Handling
Consistent Response Format
```

Do not expose MongoDB or internal server errors directly to the Android application.

---

# 18. API Response Rules

Success:

```json
{
  "success": true,
  "message": "Operation successful",
  "data": {}
}
```

Failure:

```json
{
  "success": false,
  "message": "Operation failed",
  "errors": []
}
```

Keep API responses consistent.

---

# 19. Error Handling Rules

Handle:

- Invalid input.
- Invalid token.
- Expired token.
- Unauthorized access.
- Missing resource.
- Duplicate data.
- Database errors.
- Cloudinary upload failure.
- Firebase errors.
- Network errors.

Do not leave errors silently ignored.

---

# 20. Testing Rules

Every feature must be tested before committing.

The minimum loop is:

```text
BUILD
  ↓
TEST
  ↓
FIND BUG
  ↓
FIX
  ↓
TEST AGAIN
  ↓
VERIFY
  ↓
COMMIT
```

---

# 21. Android Build Requirement

Before every stable Android commit:

```bash
./gradlew build
```

If the project has compilation errors:

```text
DO NOT COMMIT
```

Fix the issue first.

---

# 22. Backend Testing Requirement

Before every stable backend commit:

1. Start the backend.
2. Verify environment variables.
3. Test affected APIs.
4. Test validation errors.
5. Test authorization.
6. Check database results.

---

# 23. Full Integration Testing

Before considering a major phase complete:

```text
ANDROID APP
      ↓
FIREBASE AUTH
      ↓
NODE.JS BACKEND
      ↓
MONGODB
      ↓
CLOUDINARY
```

Test the complete flow.

Example:

```text
Login
  ↓
Create Profile
  ↓
Create Product
  ↓
Upload Images
  ↓
Browse Product
  ↓
Place Bid
  ↓
Accept Bid
  ↓
Create Deal
  ↓
Open Chat
  ↓
Send Message
```

---

# 24. Git Rules

Commit only stable work.

Use professional commit messages.

Format:

```text
type: short description
```

Examples:

```text
feat: implement Firebase email authentication

feat: add farmer product creation API

feat: implement product bidding flow

fix: validate product image count

fix: prevent duplicate bid acceptance

refactor: improve chat repository structure

docs: update API endpoint documentation
```

---

# 25. Git Commit Types

Use:

```text
feat
fix
refactor
docs
test
chore
```

Do not use vague messages such as:

```text
update
done
changes
final
fix bug
working
```

---

# 26. Mandatory Commit Workflow

Before committing:

```text
1. Feature implemented
2. Tests completed
3. Bugs fixed
4. Android build checked
5. Backend API tested
6. No secrets committed
7. Git status checked
8. Commit created
```

Then:

```bash
git status
git add .
git commit -m "feat: implement product creation flow"
```

---

# 27. Environment Variable Rules

Never commit:

```text
Firebase credentials
MongoDB URI
Cloudinary secret
API keys
Service account files
```

Use:

```text
.env
```

Provide:

```text
.env.example
```

The `.env.example` file must contain variable names without real secret values.

---

# 28. No Hardcoded Secrets

Never hardcode:

```text
API KEYS
PASSWORDS
DATABASE URLs
CLOUDINARY SECRETS
FIREBASE PRIVATE KEYS
```

Use environment configuration.

---

# 29. No Feature Skipping

Do not mark a feature as complete if:

- The UI exists but the API does not work.
- The API exists but the Android app is not connected.
- The happy path works but error handling is missing.
- The feature has not been tested.

A feature is complete only when:

```text
UI
+
API
+
DATABASE
+
VALIDATION
+
ERROR HANDLING
+
TESTING
```

are complete.

---

# 30. Development Communication Rule

If a required detail is missing before implementation, stop and identify the missing information.

Do not silently guess important product requirements.

However, reasonable technical defaults may be used when they do not change product behavior.

---

# 31. Completion Definition

A feature is considered complete only when:

```text
Requirement Understood
        ↓
UI Designed in Google Stitch
        ↓
Frontend Implemented
        ↓
Backend Implemented
        ↓
Database Updated
        ↓
Integration Completed
        ↓
Feature Tested
        ↓
Bugs Fixed
        ↓
Gradle Build Successful
        ↓
Committed to GitHub
```

---

# 32. Final Mandatory Development Loop

For every FarmBid feature:

```text
UNDERSTAND
   ↓
PLAN
   ↓
CHECK DOCUMENTATION
   ↓
DESIGN IN GOOGLE STITCH
   ↓
BUILD
   ↓
TEST
   ↓
FIX
   ↓
TEST AGAIN
   ↓
RUN GRADLE BUILD
   ↓
VERIFY BACKEND
   ↓
COMMIT TO GITHUB
```

**Do not skip steps.**
