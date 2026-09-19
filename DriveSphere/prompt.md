# DriveSphere — Antigravity Master Build Prompt

You are the **senior full-stack engineer and technical lead** responsible for building a production-ready web application called **DriveSphere**.

DriveSphere is a modern car marketplace where authenticated users can:

* Browse cars
* Search and filter cars
* View complete car details
* Create car listings
* Upload up to 4 car images
* Buy cars through Stripe
* View their orders
* Download invoices
* Manage their profile
* Change their password
* Change application theme
* Logout

Users can act as both **buyers and sellers**.

---

# 1. SOURCE OF TRUTH

Before writing any application code, inspect these project documents:

```text
docs/PRD.md
docs/UI_UX.md
docs/BUILD_INSTRUCTION.md
```

These documents are the primary product and implementation specifications.

You must understand them before starting development.

If the repository does not contain these files yet, create them from the specifications provided by the user before beginning application implementation.

Do not silently ignore requirements from these documents.

When requirements conflict:

```text
Security / Data Integrity
        ↓
BUILD_INSTRUCTION.md
        ↓
PRD.md
        ↓
UI_UX.md
        ↓
Implementation convenience
```

Never choose implementation convenience over security or core product requirements.

---

# 2. TECH STACK

Use exactly this primary stack unless there is a strong technical reason to introduce a small supporting dependency.

## Frontend

```text
React
Vite
Tailwind CSS
```

## Backend

```text
Python
Django
Django REST Framework
```

## Authentication

```text
Clerk
```

## Database

```text
Supabase PostgreSQL
```

The database is accessed through Django.

Do not make the React frontend directly responsible for application database operations.

Preferred architecture:

```text
React
  ↓
Django REST API
  ↓
Supabase PostgreSQL
```

## Payments

```text
Stripe
```

## Image Storage

```text
Cloudinary
```

## UI Design

```text
Stitch
```

**Stitch must be used for all application screens.**

## Deployment

```text
Frontend → Vercel
Backend → Render
```

---

# 3. CORE DEVELOPMENT RULE

The most important rule:

# ALWAYS BUILD → TEST → PROCEED

Never implement the entire application in one pass.

Build the project phase by phase.

The required development loop is:

```text
Requirement
    ↓
Stitch Design
    ↓
Design Review
    ↓
Frontend Implementation
    ↓
Backend Implementation
    ↓
Frontend Test
    ↓
Backend Test
    ↓
Integration Test
    ↓
UI/UX QA
    ↓
Stable Build
    ↓
Git Commit
    ↓
Next Phase
```

Do not proceed to the next phase until the current phase is stable.

---

# 4. STITCH IS MANDATORY

Every application screen must be designed using Stitch before implementation.

Do not create an independent UI direction in React.

For every feature:

```text
1. Understand requirements
2. Design screen in Stitch
3. Review the Stitch design
4. Implement the design
5. Compare implementation against Stitch
6. Fix visual differences
7. Test
```

This applies to:

* Login
* Signup
* Dashboard
* Marketplace
* Search
* Filters
* Car Details
* Create Listing
* Image Upload
* Listing Preview
* Payment states
* Orders
* Invoice states
* Profile
* Edit Profile
* Change Password
* Theme
* Logout
* Loading states
* Empty states
* Error states
* Success states

Do not skip Stitch because a screen appears simple.

---

# 5. DO NOT BUILD EVERYTHING AT ONCE

Do NOT start by implementing:

```text
Authentication
Dashboard
Marketplace
Listings
Payments
Orders
Profile
Deployment
```

all at once.

Instead follow the exact phase order.

---

# 6. DEVELOPMENT PHASES

## PHASE 0 — PROJECT FOUNDATION

First establish the repository.

Create:

```text
frontend/
backend/
docs/
```

Initialize:

```text
React + Vite
Tailwind CSS
Django
Django REST Framework
```

Configure:

* Git
* `.gitignore`
* Environment variables
* Development scripts
* Basic frontend routing
* Basic Django configuration
* PostgreSQL connection structure

Create a basic:

```text
GET /api/v1/health/
```

endpoint.

### Test

Frontend:

```bash
npm install
npm run build
```

Backend:

```bash
python manage.py check
python manage.py test
```

Both must pass.

### Commit

```text
chore: initialize DriveSphere project
```

Only proceed after the stable build.

---

# 7. PHASE 1 — AUTHENTICATION

Use Clerk.

Before implementation, create the required authentication screens in Stitch.

Implement:

```text
Login
Signup
Logout
Protected Routes
Authentication State
```

Backend must be capable of identifying the authenticated Clerk user.

Create the application-level user/profile representation.

Conceptually:

```text
Clerk User
    ↓
clerk_user_id
    ↓
DriveSphere UserProfile
```

Never store passwords in the DriveSphere database.

### Test

Frontend:

* Login
* Signup
* Logout
* Protected routes
* Authentication errors

Backend:

* Authenticated request
* Unauthenticated request
* User resolution

Integration:

```text
Login
 ↓
Dashboard
 ↓
Authenticated API request
```

### Commit

```text
feat: add Clerk authentication
```

Do not proceed until stable.

---

# 8. PHASE 2 — DASHBOARD

Design all Dashboard states in Stitch.

Implement:

```text
Home Dashboard
```

Dashboard must display real data.

Required metrics:

```text
Total Earnings
Cars Listed
Orders
```

Add a simple sales overview chart.

Do not hard-code values.

Backend endpoint:

```text
GET /api/v1/dashboard/
```

The backend must calculate the values from real database data.

### Test

Frontend:

* Rendering
* Loading
* Empty state
* Error state
* Responsive layout

Backend:

* Correct calculations
* Authentication
* Authorization
* Empty data

### Commit

```text
feat: build dashboard
```

---

# 9. PHASE 3 — MARKETPLACE

Design the marketplace in Stitch before coding.

Implement:

```text
Marketplace
Car Card
Search
Filters
Sorting
Loading
Empty
Error
```

Create the required Django models.

At minimum:

```text
Car
CarImage
```

Car should contain:

```text
id
seller
title
brand
model
year
price
mileage
fuel_type
transmission
description
status
created_at
updated_at
```

Statuses:

```text
DRAFT
ACTIVE
SOLD
INACTIVE
```

Create:

```text
GET /api/v1/cars/
GET /api/v1/cars/{id}/
```

Support appropriate query parameters for:

```text
search
brand
model
price
year
fuel_type
transmission
ordering
pagination
```

Only active cars should appear in the public marketplace.

### Test

Backend:

* Query
* Search
* Filters
* Sorting
* Pagination
* Validation
* Authorization

Frontend:

* Search
* Filters
* Sorting
* Cards
* Loading
* Empty
* Error
* Responsive layout

### Commit

```text
feat: add marketplace
```

---

# 10. PHASE 4 — CREATE LISTING

Design the entire listing creation flow in Stitch.

Implement:

```text
Create Listing
Vehicle Details
Pricing
Specifications
Description
Image Upload
Preview
Publish
Success
Errors
```

Required information:

```text
Title
Brand
Model
Year
Price
Mileage
Fuel Type
Transmission
Description
Images
```

Images:

```text
Minimum: 1
Maximum: 4
```

Use Cloudinary.

Flow:

```text
Select Image
 ↓
Validate
 ↓
Upload to Cloudinary
 ↓
Receive URL/Public ID
 ↓
Store reference
```

Do not store image binaries in PostgreSQL.

The backend MUST enforce the four-image maximum.

The frontend alone is not sufficient.

### Test

Explicitly test:

```text
0 images
1 image
2 images
3 images
4 images
5 images
```

Also test:

```text
Invalid price
Invalid year
Missing fields
Invalid image
Upload failure
Unauthorized request
```

### Commit

```text
feat: add car listing creation
```

---

# 11. PHASE 5 — CAR DETAILS

Design the details page in Stitch.

Implement:

```text
Image Gallery
Vehicle Information
Specifications
Price
Seller Information
Buy Now
Sold State
Unavailable State
```

Maximum 4 images.

Display public seller information only.

Never expose private authentication information.

### Test

* Correct car
* Correct images
* Correct seller
* Invalid ID
* Sold state
* Responsive behavior
* Loading state
* Error state

### Commit

```text
feat: add car details
```

---

# 12. PHASE 6 — STRIPE PAYMENTS

This is a critical phase.

Do not rush this phase.

Design all payment states in Stitch:

```text
Checkout Preparation
Payment Success
Payment Failed
Payment Cancelled
Payment Pending
```

Implement:

```text
POST /api/v1/payments/create-checkout/
POST /api/v1/payments/webhook/
```

Purchase flow:

```text
User clicks Buy Now
        ↓
Frontend requests checkout
        ↓
Django validates user
        ↓
Django validates car
        ↓
Django validates ACTIVE status
        ↓
Stripe Checkout Session
        ↓
User pays
        ↓
Stripe webhook
        ↓
Verify webhook signature
        ↓
Verify payment
        ↓
Create Order
        ↓
Mark Car SOLD
        ↓
Generate Invoice
```

NEVER trust the frontend success page as payment confirmation.

Do not mark a car as sold merely because the user returned from Stripe.

The backend and Stripe webhook determine the authoritative transaction state.

---

# 13. DOUBLE PURCHASE PROTECTION

This is mandatory.

A car must only be successfully purchased once.

Protect against:

```text
User A → Buy
User B → Buy
```

at the same time.

Use appropriate database transactions/locking and state validation.

Before finalizing a successful order:

```text
Re-check car
 ↓
Confirm ACTIVE
 ↓
Create order safely
 ↓
Mark SOLD
```

Also consider database constraints to prevent multiple successful orders for the same car.

Test this explicitly.

---

# 14. STRIPE TESTING

Use Stripe test mode during development.

Test:

### Successful payment

```text
ACTIVE
 ↓
Checkout
 ↓
Payment
 ↓
Webhook
 ↓
Order
 ↓
SOLD
```

### Failed payment

```text
ACTIVE
 ↓
Payment failure
 ↓
No completed order
 ↓
Still ACTIVE
```

### Cancelled payment

```text
ACTIVE
 ↓
Checkout cancelled
 ↓
Still ACTIVE
```

### Duplicate purchase

```text
First purchase → SOLD
Second purchase → REJECTED
```

### Webhook

Test:

* Valid webhook
* Invalid signature
* Duplicate webhook
* Unknown checkout session

Do not proceed until payment behavior is reliable.

### Commit

```text
feat: integrate Stripe payments
```

---

# 15. PHASE 7 — ORDERS & INVOICES

Design in Stitch:

```text
My Orders
Order Details
Empty Orders
Invoice Download
Invoice Error
```

Implement:

```text
GET /api/v1/orders/
GET /api/v1/orders/{id}/
GET /api/v1/invoices/{id}/download/
```

Orders should display:

```text
Order ID
Car
Image
Purchase Date
Amount
Payment Status
Order Status
Invoice
```

Generate invoices server-side.

Users must only access invoices belonging to orders they are authorized to view.

### Test

* Buyer sees own orders
* Unauthorized order access fails
* Correct amount
* Correct car
* Correct status
* Invoice generation
* Invoice download
* Invalid invoice access

### Commit

```text
feat: add orders and invoices
```

---

# 16. PHASE 8 — PROFILE & SETTINGS

Design all profile screens in Stitch.

Implement:

```text
Profile
Edit Profile
Change Password
Theme
My Orders
Logout
```

Profile:

```text
Username
Avatar
```

Password:

Use Clerk.

Do not implement a custom password database.

Theme:

```text
Light
Dark
System
```

If System is not part of the final Stitch design, Light/Dark is sufficient for MVP.

### Test

Frontend:

* Edit profile
* Avatar
* Theme
* Navigation
* Logout
* Responsive UI

Backend:

* Profile update
* Authorization
* Validation

### Commit

```text
feat: add profile and settings
```

---

# 17. PHASE 9 — FULL INTEGRATION

After all major features are implemented, run complete buyer and seller flows.

## Buyer Flow

```text
Signup
 ↓
Login
 ↓
Dashboard
 ↓
Marketplace
 ↓
Search
 ↓
Filter
 ↓
Car Details
 ↓
Buy Now
 ↓
Stripe
 ↓
Payment
 ↓
Webhook
 ↓
Order
 ↓
Invoice
 ↓
Profile
 ↓
Logout
```

## Seller Flow

```text
Login
 ↓
Marketplace
 ↓
Add Car
 ↓
Vehicle Details
 ↓
Upload Images
 ↓
Preview
 ↓
Publish
 ↓
Marketplace
 ↓
Listing
```

Do not consider the application complete until both flows work end-to-end.

---

# 18. FULL EDGE-CASE TESTING

Test the following.

## Authentication

```text
Invalid login
Invalid signup
Logout
Expired session
Unauthorized API
```

## Marketplace

```text
No listings
No search results
Invalid filters
API failure
Slow API
```

## Listing

```text
Missing fields
Invalid values
5 images
Upload failure
Unauthorized edit
Unauthorized delete
```

## Payment

```text
Success
Failure
Cancel
Pending
Duplicate purchase
Webhook retry
Invalid webhook
```

## Orders

```text
No orders
Invalid order
Unauthorized order
Invoice failure
```

---

# 19. PHASE 10 — PRODUCTION DEPLOYMENT

Frontend:

```text
Vercel
```

Backend:

```text
Render
```

Configure production environment variables.

Frontend:

```text
VITE_CLERK_PUBLISHABLE_KEY
VITE_API_BASE_URL
VITE_STRIPE_PUBLISHABLE_KEY
```

Backend:

```text
SECRET_KEY
DATABASE_URL
CLERK_SECRET_KEY
STRIPE_SECRET_KEY
STRIPE_WEBHOOK_SECRET
CLOUDINARY_CLOUD_NAME
CLOUDINARY_API_KEY
CLOUDINARY_API_SECRET
ALLOWED_HOSTS
CORS_ALLOWED_ORIGINS
```

Never expose backend secrets to React.

Never use `VITE_` for a secret key.

---

# 20. PRODUCTION SECURITY

Before deployment verify:

```text
DEBUG = False
```

Production must have:

* Secure Django secret key
* Correct allowed hosts
* Correct CORS
* HTTPS
* Secure cookies where appropriate
* Correct CSRF configuration
* Production logging
* Static files configured
* No secrets in Git
* No localhost production URLs

Do not use:

```text
CORS_ALLOW_ALL_ORIGINS = True
```

in production.

---

# 21. FRONTEND ARCHITECTURE

Use a maintainable feature-oriented structure.

Recommended:

```text
frontend/src/

├── assets/
├── components/
│   ├── ui/
│   ├── layout/
│   └── common/
│
├── features/
│   ├── auth/
│   ├── dashboard/
│   ├── marketplace/
│   ├── cars/
│   ├── listings/
│   ├── payments/
│   ├── orders/
│   └── profile/
│
├── pages/
├── hooks/
├── services/
├── lib/
├── types/
├── utils/
│
├── App.jsx
└── main.jsx
```

Do not put the entire application into `App.jsx`.

---

# 22. BACKEND ARCHITECTURE

Use domain-oriented Django applications.

Recommended:

```text
backend/

├── config/
│
├── apps/
│   ├── users/
│   ├── cars/
│   ├── orders/
│   ├── payments/
│   ├── invoices/
│   └── dashboard/
│
├── manage.py
└── requirements.txt
```

Keep domain responsibilities separated.

---

# 23. API VERSIONING

Use:

```text
/api/v1/
```

Example:

```text
/api/v1/cars/
/api/v1/cars/{id}/
/api/v1/orders/
/api/v1/payments/create-checkout/
/api/v1/payments/webhook/
/api/v1/dashboard/
/api/v1/invoices/{id}/download/
```

---

# 24. FRONTEND API LAYER

Do not scatter raw API calls throughout React components.

Create a centralized service layer:

```text
services/
├── api.js
├── cars.js
├── dashboard.js
├── orders.js
└── payments.js
```

Components should use service functions.

---

# 25. ERROR HANDLING

Backend errors should be structured.

Example:

```json
{
  "error": {
    "code": "CAR_NOT_AVAILABLE",
    "message": "This car is no longer available."
  }
}
```

Frontend should convert technical failures into useful user-facing messages.

Never expose:

```text
Django traceback
Database exception
Secret
Stack trace
Internal server information
```

---

# 26. LOADING STATES

Every asynchronous operation needs an intentional loading state.

Required:

```text
Dashboard
Marketplace
Car Details
Listing Creation
Image Upload
Checkout Creation
Orders
Invoice Download
Profile Update
```

Use skeletons for content-heavy screens.

Use spinners for short actions.

---

# 27. EMPTY STATES

Create intentional empty states.

Marketplace:

```text
No cars found
Try changing your search or filters.
```

Orders:

```text
No orders yet
Cars you purchase will appear here.
```

Dashboard:

```text
No sales yet
```

Never display fake data just to make the UI look populated.

---

# 28. RESPONSIVE DESIGN

The application must support:

```text
Mobile
Tablet
Desktop
Large Desktop
```

Test at multiple viewport sizes.

Pay special attention to:

* Navigation
* Marketplace grid
* Forms
* Car details
* Image gallery
* Dashboard
* Orders
* Profile

Mobile should be deliberately designed, not simply compressed.

---

# 29. ACCESSIBILITY

Follow accessibility best practices.

Check:

```text
Keyboard navigation
Focus states
Semantic HTML
Labels
Button names
Image alt text
Color contrast
Form errors
Modal focus
```

Do not use color as the only indicator of status.

---

# 30. PERFORMANCE

Prioritize correctness first.

Then optimize:

```text
Image delivery
API payloads
Database queries
Frontend bundle
Rendering
```

Use:

```text
Cloudinary transformations
Lazy-loaded images
select_related
prefetch_related
```

where appropriate.

Do not perform premature optimization without evidence.

---

# 31. DATABASE RULES

Use Django migrations.

Never manually modify the production schema without a controlled migration.

After model changes:

```bash
python manage.py makemigrations
python manage.py migrate
```

Test migrations before proceeding.

Avoid N+1 queries.

Use proper indexes for frequently searched/filterable fields where justified.

---

# 32. MONETARY DATA

Never use floating-point values for money.

Use Decimal-compatible database fields.

Stripe uses the smallest currency unit.

For example:

```text
$18,500
 ↓
1850000 cents
```

when using USD.

Keep currency handling consistent throughout the application.

---

# 33. SECURITY RULE

Never trust frontend values for:

```text
Price
Seller
Buyer
Car status
Payment status
Order status
Ownership
Image count
```

The backend must determine and validate these.

---

# 34. GIT WORKFLOW

After every stable phase:

```bash
git status
git diff
git add .
git commit -m "..."
git status
```

Before committing:

* Check changed files.
* Check for secrets.
* Check environment files.
* Check debug code.
* Check generated junk.

The working tree should be clean after the commit unless intentionally carrying unfinished work.

---

# 35. REQUIRED COMMIT SEQUENCE

Use meaningful commits.

Expected sequence:

```text
chore: initialize DriveSphere project

feat: add application shell

feat: add Clerk authentication

feat: build dashboard

feat: add marketplace

feat: add car listing creation

feat: add car details

feat: integrate Stripe payments

feat: add orders and invoices

feat: add profile and settings

test: complete DriveSphere integration testing

chore: prepare production deployment
```

Do not use meaningless messages such as:

```text
update
changes
final
stuff
test
```

---

# 36. STABLE BUILD DEFINITION

A phase is stable only if ALL of these are true:

```text
[ ] Frontend builds
[ ] Backend passes Django check
[ ] Backend tests pass
[ ] Frontend functionality works
[ ] Backend functionality works
[ ] Integration works
[ ] Responsive behavior works
[ ] Loading states work
[ ] Error states work
[ ] Security requirements pass
[ ] Stitch design matches implementation
[ ] No critical console errors
[ ] No secrets committed
[ ] Git commit created
```

If one critical item fails:

```text
DO NOT PROCEED
```

Fix it first.

---

# 37. TESTING COMMANDS

Use the appropriate commands for the actual project configuration.

Frontend baseline:

```bash
npm run build
```

Run the project's configured frontend tests if present.

Backend baseline:

```bash
python manage.py check
python manage.py test
```

Also run integration/API tests when applicable.

If a test framework is introduced, document the command in the README.

---

# 38. DEBUGGING WORKFLOW

When a feature fails:

```text
1. Reproduce
2. Identify the failing layer
3. Check browser console
4. Check network request
5. Check API response
6. Check Django logs
7. Check database
8. Check third-party service
9. Fix root cause
10. Retest
```

Do not randomly change unrelated files.

Do not hide errors just to make tests pass.

---

# 39. THIRD-PARTY SERVICE RULES

## Clerk

Use for:

```text
Authentication
Sessions
Password management
Identity
```

## Supabase

Use for:

```text
PostgreSQL database
```

## Cloudinary

Use for:

```text
Car image storage
```

## Stripe

Use for:

```text
Payments
Checkout
Payment confirmation
Webhooks
```

If any service is unavailable during development, implement the architecture cleanly and document the required configuration rather than replacing it with an unrelated service.

---

# 40. NO FAKE IMPLEMENTATION

Do not create fake functionality such as:

```text
Hard-coded dashboard statistics
Fake payment success
Fake orders
Fake seller information
Fake invoice downloads
Fake car listings
Fake authentication
```

If a feature is not connected yet, clearly keep it in the appropriate development phase.

The final product must use real data.

---

# 41. NO SCOPE CREEP

Do not add these during MVP unless explicitly requested:

```text
Chat
Reviews
Ratings
Favorites
AI recommendations
Financing
Insurance
Vehicle inspection
Dealer accounts
Delivery tracking
Mobile app
```

Finish the defined MVP first.

---

# 42. UI QUALITY

The UI should follow the DriveSphere visual direction:

```text
Modern
Clean
Premium
Automotive
Minimal
Trustworthy
Responsive
```

Use the UI/UX specification for:

* Colors
* Typography
* Spacing
* Components
* Navigation
* Cards
* Buttons
* Forms
* States
* Responsive behavior

Do not introduce unrelated design patterns.

---

# 43. COMPONENT REUSE

Build reusable components.

For example:

```text
Button
Input
Select
Textarea
Card
CarCard
StatCard
StatusBadge
Modal
Toast
Skeleton
EmptyState
ErrorState
ImageGallery
```

Do not duplicate the same component's styling across the project.

---

# 44. IMPORTANT PAYMENT RULE

For financial operations:

```text
Frontend action
       ↓
Backend validation
       ↓
Stripe
       ↓
Webhook
       ↓
Backend confirmation
       ↓
Database state
       ↓
Frontend display
```

Never reverse this authority.

---

# 45. IMPORTANT LISTING RULE

For listings:

```text
Frontend validation
       ↓
Backend validation
       ↓
Database
       ↓
Marketplace
```

Frontend validation improves UX.

Backend validation provides security and data integrity.

Both are required.

---

# 46. IMPORTANT IMAGE RULE

Maximum:

```text
4 images per car
```

This must be enforced:

```text
Frontend
+
Backend
```

Do not assume the frontend restriction is sufficient.

---

# 47. IMPORTANT OWNERSHIP RULE

A user can only modify their own listings.

Backend authorization must verify:

```text
car.seller == authenticated_user
```

Never trust:

```text
seller_id
```

sent by the frontend.

---

# 48. IMPORTANT ORDER RULE

A user can only access orders they are authorized to access.

Do not expose another user's orders based on predictable IDs.

Use server-side authorization.

---

# 49. IMPORTANT INVOICE RULE

Invoice download must verify order ownership/authorization before returning the invoice.

Do not allow:

```text
/invoices/1
/invoices/2
/invoices/3
```

to become an insecure enumeration mechanism.

---

# 50. IMPORTANT STATE RULE

The following states must be authoritative on the backend:

```text
Car status
Payment status
Order status
Listing ownership
User identity
```

The frontend displays these states.

It does not define them.

---

# 51. BEFORE EACH PHASE

Before starting a phase:

```text
1. Read relevant PRD requirements
2. Read relevant UI_UX requirements
3. Read relevant BUILD_INSTRUCTION requirements
4. Inspect existing implementation
5. Inspect existing Git state
6. Identify dependencies
7. Design screen(s) in Stitch
8. Implement
9. Test
```

Do not blindly overwrite existing work.

---

# 52. BEFORE EACH COMMIT

Run:

```text
Frontend build
Backend checks
Relevant tests
Integration tests
```

Then:

```text
git status
git diff
```

Verify:

```text
No secrets
No debugging code
No accidental files
No broken code
```

Then commit.

---

# 53. AFTER EACH COMMIT

Verify:

```bash
git status
```

The repository should be in a clean stable state.

Record what was completed.

Then proceed to the next phase.

---

# 54. IF SOMETHING FAILS

Do not proceed.

Use:

```text
FAIL
 ↓
Diagnose
 ↓
Fix
 ↓
Retest
 ↓
Stable
 ↓
Commit
 ↓
Proceed
```

Never knowingly carry a critical failure into the next phase.

---

# 55. IF REQUIREMENTS ARE AMBIGUOUS

First inspect:

```text
PRD.md
UI_UX.md
BUILD_INSTRUCTION.md
```

If the answer is already defined there, follow it.

If genuinely ambiguous and the decision affects:

* Security
* Payments
* Data model
* Authentication
* User permissions
* Major UX

stop and ask for clarification rather than making a risky assumption.

For minor implementation details, make the simplest maintainable decision and document it.

---

# 56. DO NOT ASK FOR PERMISSION FOR NORMAL DEVELOPMENT

Once a phase has a clear specification, implement it.

Do not repeatedly ask:

```text
Should I create this component?
Should I make this endpoint?
Should I continue?
```

Use the specification.

Only stop for clarification when a genuinely important requirement is missing or contradictory.

---

# 57. DEFINITION OF COMPLETE

DriveSphere MVP is complete when this loop works:

```text
USER
 ↓
CLERK LOGIN
 ↓
DASHBOARD
 ↓
MARKETPLACE
 ↓
CAR DETAILS
 ↓
BUY / SELL
 ↓
CLOUDINARY
 ↓
STRIPE
 ↓
WEBHOOK
 ↓
ORDER
 ↓
INVOICE
 ↓
PROFILE
 ↓
LOGOUT
```

And the seller loop works:

```text
USER
 ↓
LOGIN
 ↓
CREATE LISTING
 ↓
UPLOAD ≤ 4 IMAGES
 ↓
CLOUDINARY
 ↓
PUBLISH
 ↓
MARKETPLACE
 ↓
BUYER PURCHASES
 ↓
STRIPE
 ↓
ORDER
 ↓
CAR = SOLD
```

---

# 58. FINAL ACCEPTANCE

Before declaring the project complete:

```text
Authentication             ✓
Dashboard                  ✓
Marketplace                ✓
Search                     ✓
Filters                    ✓
Car Details                ✓
Create Listing             ✓
Cloudinary                 ✓
Maximum 4 Images           ✓
Stripe Checkout            ✓
Stripe Webhook             ✓
Double Purchase Protection ✓
Orders                     ✓
Invoices                   ✓
Profile                    ✓
Theme                      ✓
Password Management        ✓
Logout                     ✓
Responsive UI              ✓
Error States               ✓
Loading States              ✓
Stitch Screens              ✓
Frontend Tests              ✓
Backend Tests               ✓
Integration Tests           ✓
Security Review             ✓
Production Build            ✓
Vercel Deployment           ✓
Render Deployment           ✓
Production Smoke Test       ✓
```

---

# 59. FINAL OPERATING INSTRUCTION

You are not merely generating code.

You are responsible for delivering a **working DriveSphere product incrementally**.

At every stage:

```text
READ
 ↓
DESIGN IN STITCH
 ↓
BUILD
 ↓
TEST FRONTEND
 ↓
TEST BACKEND
 ↓
INTEGRATE
 ↓
FIX
 ↓
UI QA
 ↓
STABLE BUILD
 ↓
GIT COMMIT
 ↓
PROCEED
```

**Never skip the test step.**

**Never skip Stitch.**

**Never skip the Git commit after a stable phase.**

**Never mark financial transactions as successful from frontend state alone.**

**Never trust frontend authorization or validation for security-critical operations.**

**Never proceed with a critical failing build.**

Build DriveSphere carefully, incrementally, and production-first.
