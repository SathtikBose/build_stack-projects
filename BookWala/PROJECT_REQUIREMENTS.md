# BookWala — Project Requirements

## 1. Project Overview

**Project Name:** BookWala  
**Android Package Name:** `com.buildstack.bookwala`

BookWala is a second-hand book marketplace designed primarily for students.

The application allows users to:

- Buy second-hand books from other users.
- Sell their own used books.
- Browse and search available books.
- View detailed book information.
- Contact a seller directly through real-time one-to-one chat.
- See seller online/offline status and last seen.
- Use an AI-powered book discovery assistant to describe what type of book they need.
- Receive matching books from the BookWala marketplace with direct access to the relevant listing.

The application should feel like a **real production marketplace**, not a demo, tutorial project, or generic CRUD application.

---

# 2. Product Vision

BookWala should make it easy for students to find affordable second-hand books and sell books they no longer need.

The core experience should be:

```text
Discover
   ↓
Find a Book
   ↓
View Book Details
   ↓
Contact Seller
   ↓
Discuss / Confirm
   ↓
Complete the Purchase Process
```

For sellers:

```text
Have an Unused Book
   ↓
Create Listing
   ↓
Upload Book Information + Image
   ↓
Publish
   ↓
Receive Buyer Messages
   ↓
Sell the Book
```

For AI-assisted discovery:

```text
User describes required book
        ↓
AI understands requirement
        ↓
Search BookWala marketplace
        ↓
Find matching listings
        ↓
Rank relevant results
        ↓
Show book recommendations
        ↓
User opens listing
        ↓
Contact seller
```

---

# 3. Target Users

## 3.1 Primary Users

Students who want to:

- Buy affordable second-hand books.
- Sell books they no longer need.
- Search for specific editions or authors.
- Find books based on subject, genre, type, or requirements.

## 3.2 Secondary Users

Any user who wants to buy or sell used books through the marketplace.

---

# 4. Core Product Principles

BookWala must follow these principles throughout development:

1. **Simple**
   - Users should understand the application without instructions.

2. **Fast**
   - Marketplace browsing and search should feel responsive.

3. **Trustworthy**
   - Clearly display seller information and listing information.

4. **Real marketplace experience**
   - The application must not look like a CRUD demo.

5. **Mobile-first**
   - The primary experience is Android.

6. **Production-oriented architecture**
   - Keep UI, business logic, networking, data, and real-time functionality properly separated.

7. **AI should assist discovery**
   - AI should help users find marketplace listings.
   - AI should not invent unavailable books or listings.

8. **User-controlled communication**
   - Buyers and sellers communicate directly through the application's real-time chat.

---

# 5. Technology Stack

## 5.1 Android

- Kotlin
- Jetpack Compose

The Android application should use modern Android architecture and Compose-based UI.

Recommended architectural separation:

```text
UI
 ↓
ViewModel / State
 ↓
Use Cases / Domain Logic
 ↓
Repository
 ↓
Remote / Local Data Sources
```

The exact architecture may be refined during implementation, but responsibilities must remain clearly separated.

---

## 5.2 Backend

- Node.js
- Express.js

The backend is responsible for:

- User/application data
- Book listings
- Marketplace APIs
- Search
- Listing management
- AI-related marketplace search
- Business validation
- Communication between Android client and database/services

---

## 5.3 Authentication

Firebase Authentication:

- Email + Password
- Google Sign-In

Authentication must be integrated with the Android application and backend securely.

---

## 5.4 Database

MongoDB

MongoDB will store application data such as:

- Users
- Book listings
- Chat-related metadata
- Messages or message-related application data where applicable
- Marketplace information
- Other required application entities

Database schema must be designed before implementation of dependent backend features.

---

## 5.5 Real-Time Communication

WebSocket-based real-time communication.

Required functionality:

- One-to-one chat
- Real-time messages
- Online status
- Offline status
- Last seen
- Connection/disconnection handling

Firebase real-time capabilities may be used where appropriate for the defined messaging architecture, but WebSocket remains the real-time communication requirement for the buyer-seller chat experience.

---

## 5.6 Image Storage

Cloudinary

Cloudinary will be used for:

- Book images
- Image upload
- Image hosting
- Image URL management

The application should not unnecessarily store large image files directly in MongoDB.

---

## 5.7 Backend Hosting

Render

The Node.js/Express backend will be deployed to Render.

Production configuration must keep secrets and environment variables outside source code.

---

# 6. Android Package

The Android package name must be:

```text
com.buildstack.bookwala
```

Do not change the package name without explicit approval.

---

# 7. Authentication Requirements

## 7.1 Authentication Options

Users must be able to authenticate using:

### Email and Password

- Register
- Login
- Logout

### Google

- Continue with Google
- Authenticate through Firebase Authentication

---

# 8. Application Entry Flow

```text
App Launch
    ↓
Splash / Initial Loading
    ↓
Check Authentication State
    ↓
┌─────────────────────┐
│                     │
Authenticated      Not Authenticated
│                     │
↓                     ↓
Home              Authentication
                      │
                ┌─────┴─────┐
                ↓           ↓
              Login      Register
                │
                └─────→ Home
```

The authentication state must be persisted appropriately so authenticated users do not unnecessarily see the login screen every time they launch the application.

---

# 9. Main Application Areas

The application should contain the following major areas:

1. Marketplace / Home
2. Search
3. Book Details
4. Create Listing
5. My Listings
6. Chat
7. AI Book Assistant
8. Profile
9. Settings
10. Authentication

The final navigation structure should be determined during UI design, but all required functionality must remain accessible.

---

# 10. Marketplace

The Marketplace is the primary discovery area of BookWala.

Users should be able to:

- Browse available books.
- Search books.
- Filter books.
- Sort results.
- Open a book listing.
- View book details.
- Contact the seller.

Marketplace listings should communicate important information quickly:

- Book image
- Book name
- Author
- Edition
- Price
- Relevant short information
- Seller information where appropriate

---

# 11. Book Listing

A seller must be able to create a second-hand book listing.

## 11.1 Required Listing Information

The listing must support:

### Book Image

User uploads an image of the book.

The image should be uploaded to Cloudinary.

### Book Name

The name/title of the book.

### Edition

The edition of the book.

### Author

The book author.

### Price

The asking price for the second-hand book.

### Additional Description

Optional additional information from the seller.

Examples:

- Book condition
- Notes/highlights
- Damage
- Used duration
- Additional details

The application should not unnecessarily restrict the seller's description.

---

# 12. Create Listing Flow

```text
My Listings
    ↓
Create New Listing
    ↓
Add Book Image
    ↓
Enter Book Name
    ↓
Enter Edition
    ↓
Enter Author
    ↓
Enter Price
    ↓
Enter Additional Description
    ↓
Preview Listing
    ↓
Publish
    ↓
Listing Appears in Marketplace
```

Before publishing:

- Required fields must be validated.
- Image upload must be handled properly.
- Invalid prices must be rejected.
- Empty required information must be identified clearly.
- Network errors must not silently publish an incomplete listing.

---

# 13. Listing Management

Users must be able to manage books they have listed.

Required functionality:

- View my listings
- Open listing
- Edit listing
- Delete listing

The UI should clearly distinguish the user's own listings from other marketplace listings.

---

# 14. Marketplace Search

Users should be able to search for books.

Search should support useful marketplace information such as:

- Book name
- Author
- Subject/category where available
- Genre where applicable
- Edition
- Relevant keywords

Search should return marketplace listings rather than external books unless a future requirement explicitly introduces external sources.

---

# 15. Filtering

The marketplace should support filtering where appropriate.

Potential filters include:

- Price
- Author
- Edition
- Category / genre
- Book type
- Availability

Filters should be implemented based on actual stored listing data.

Do not expose filters for data that does not exist in the backend.

---

# 16. Sorting

Marketplace results should support useful sorting options where appropriate.

Examples:

- Price: Low to High
- Price: High to Low
- Newest Listings

Additional sorting can be added later if justified.

---

# 17. Book Details Screen

Selecting a marketplace listing must open a dedicated Book Details screen.

The screen should display:

- Book image
- Book name
- Author
- Edition
- Price
- Description
- Seller information
- Listing information
- Contact / purchase-intent action

The primary buyer action should be:

**I Want to Buy**

---

# 18. Buyer Flow

```text
Marketplace
    ↓
Search / Browse
    ↓
Book Listing
    ↓
Book Details
    ↓
I Want to Buy
    ↓
One-to-One Chat
    ↓
Buyer ↔ Seller
    ↓
Discuss Purchase
    ↓
Confirm Details
    ↓
Complete Purchase Process
```

BookWala's initial scope does not require an integrated payment gateway unless explicitly added as a future requirement.

The buyer and seller should be able to communicate and complete their process through the application.

---

# 19. One-to-One Chat

When a user selects **I Want to Buy**, the application should open a one-to-one conversation between:

```text
Buyer ↔ Seller
```

The conversation should be associated with the relevant book listing.

The buyer should not accidentally start a conversation with the wrong seller or unrelated listing.

---

# 20. Chat Requirements

The chat system must support:

- Real-time messages
- Buyer/seller identification
- Message timestamps
- Online status
- Offline status
- Last seen
- Connection state handling
- Sending messages
- Receiving messages
- Conversation loading

The chat must feel real-time rather than requiring manual refresh.

---

# 21. Online / Offline / Last Seen

The application should communicate seller/buyer presence where appropriate.

Possible states:

```text
Online
Offline
Last seen recently
Last seen at <time>
```

Presence state must not be falsely displayed.

If the real-time connection is unavailable, the application should gracefully handle the state.

---

# 22. Chat Error Handling

The application must handle:

- WebSocket connection failure
- Temporary network loss
- Message sending failure
- Reconnection
- Duplicate message prevention where applicable
- Authentication failure
- Invalid conversation
- Seller/listing no longer available

The user should receive meaningful UI feedback rather than silent failure.

---

# 23. AI Book Assistant

BookWala will contain an AI-powered book discovery assistant.

The user should be able to describe what type of book they want using natural language.

Example:

```text
"I want a second-hand Java programming book
for beginners."
```

The AI should understand the request and search BookWala marketplace listings.

---

# 24. AI Input

The user may describe:

- Genre
- Book type
- Subject
- Author preference
- Edition preference
- Skill level
- Intended use
- Other requirements

Example:

```text
"I need a beginner-friendly Python book
for college students under ₹500."
```

The AI should extract useful search requirements.

---

# 25. AI Marketplace Search

The AI workflow should be:

```text
User Query
    ↓
AI Understanding
    ↓
Extract Search Requirements
    ↓
Marketplace Search
    ↓
Find Matching Listings
    ↓
Rank Relevant Results
    ↓
Return Recommendations
```

The AI must use actual BookWala marketplace data.

It must not fabricate:

- Book listings
- Prices
- Sellers
- Availability
- Editions
- Product links

---

# 26. AI Results

AI results should provide useful book recommendations.

Each recommendation should allow the user to:

- View book
- See title
- See author
- See edition
- See price
- See relevant information
- Open the actual BookWala listing

The primary action should lead directly to the corresponding Book Details screen.

---

# 27. AI Safety / Reliability Rules

The AI agent must:

1. Never claim that an unavailable listing exists.
2. Never invent a price.
3. Never invent a seller.
4. Never fabricate a listing URL.
5. Only recommend marketplace listings returned by the backend.
6. Clearly handle situations where no matching books are found.
7. Explain when the marketplace has insufficient matching results.

Example:

```text
No exact matches found.

Here are 3 books that are close to your requirements.
```

---

# 28. Profile

Users should have a profile area.

The profile should contain appropriate user information and access to:

- My Listings
- My Chats
- Account information
- Settings
- Logout

The exact profile fields should be finalized during UI and database design.

---

# 29. User Data

A user account may contain information such as:

```text
User
├── ID
├── Name
├── Email
├── Profile Image (if implemented)
├── Authentication Provider
├── Created At
└── Updated At
```

Only collect information that is required for the application.

---

# 30. Navigation

The navigation system must be designed around the primary actions.

A likely structure is:

```text
Home
├── Marketplace
├── Search
├── AI Assistant
├── My Listings
├── Chats
└── Profile
```

The exact navigation UI must be determined from the approved Stitch design.

---

# 31. UI / UX Requirements

The application must **not look like a demo application**.

The design should feel:

- Professional
- Modern
- Consistent
- Trustworthy
- Marketplace-oriented
- Student-friendly
- Clean
- Production-ready

Avoid:

- Generic Material demo layouts
- Unstyled forms
- Excessive cards
- Random colors
- Inconsistent spacing
- Placeholder-looking screens
- Default Android-looking screens without customization
- Inconsistent typography

---

# 32. Stitch MCP Requirement

The UI must be designed using **Stitch MCP**.

The implementation workflow is:

```text
Requirement
    ↓
Stitch MCP Design
    ↓
Review Design
    ↓
Approve Design
    ↓
Implement in Jetpack Compose
    ↓
Compare Implementation Against Design
    ↓
Fix Differences
    ↓
Approve
```

The approved Stitch design is the source of truth for the UI.

The Android implementation should reproduce:

- Layout
- Spacing
- Typography
- Components
- Visual hierarchy
- Buttons
- Input fields
- Cards
- Navigation
- States
- Empty states
- Loading states
- Error states

Do not independently redesign an approved screen during implementation.

---

# 33. UI States

Every important screen should consider:

## Loading

Display an appropriate loading state while data is being retrieved.

## Success

Display actual application data.

## Empty

Examples:

```text
No books listed yet.
No matching books found.
No chats yet.
You haven't listed any books.
```

## Error

Provide understandable error messages and a recovery action where possible.

## Offline

The application should handle network loss gracefully.

---

# 34. Book Image Requirements

Book images must be handled through Cloudinary.

Basic flow:

```text
Android
   ↓
Select / Capture Book Image
   ↓
Upload
   ↓
Cloudinary
   ↓
Image URL
   ↓
Backend
   ↓
MongoDB stores image reference
```

The application should optimize images appropriately for mobile use.

---

# 35. Backend Responsibilities

The Node.js + Express backend should provide APIs for:

- User/application data
- Book listings
- Book search
- Book filtering
- Book details
- Listing creation
- Listing updates
- Listing deletion
- Marketplace queries
- AI marketplace search
- Other required application operations

Backend responsibilities must be separated into appropriate modules.

Avoid placing the entire backend application inside a single file.

---

# 36. API Design Principles

APIs should:

- Use appropriate HTTP methods.
- Validate input.
- Validate authentication.
- Return consistent responses.
- Return useful error information.
- Avoid exposing sensitive information.
- Handle invalid IDs.
- Handle missing records.
- Handle unauthorized actions.

Example conceptual API structure:

```text
/api/auth
/api/users
/api/books
/api/chats
/api/messages
/api/ai
```

The final API structure should be documented in `API_SPECIFICATION.md`.

---

# 37. Database Requirements

MongoDB should be designed around the application's actual entities.

Initial entities:

```text
Users
Books
Chats
Messages
```

Additional collections may be introduced if required by implementation.

Database relationships and indexes must be planned before implementing complex marketplace search.

---

# 38. Book Listing Ownership

A book listing must have an identifiable seller.

A user must only be able to:

- Edit their own listings.
- Delete their own listings.

A user must not be able to modify another user's listing.

Authorization must be enforced server-side.

---

# 39. Chat Authorization

A user should only be able to access conversations they are a participant in.

For a buyer-seller conversation:

```text
Chat
├── Buyer
├── Seller
├── Book
└── Messages
```

Unauthorized users must not be able to access the conversation.

---

# 40. Security Requirements

Security must be considered throughout implementation.

Required principles:

- Never hard-code API secrets.
- Never hard-code Cloudinary secrets.
- Never expose private keys in the Android application.
- Validate all backend input.
- Authenticate protected API requests.
- Authorize resource ownership.
- Sanitize/validate user-generated data.
- Protect WebSocket connections.
- Do not trust client-side authorization.
- Use environment variables for server secrets.

---

# 41. Error Handling

The application must gracefully handle:

- No internet connection
- Backend unavailable
- MongoDB failure
- Cloudinary upload failure
- Authentication failure
- WebSocket failure
- Invalid user input
- Missing book
- Deleted book
- Unauthorized operation
- AI failure
- Empty search results

Errors should be visible to the user in a clear and actionable way.

---

# 42. Performance Requirements

The application should:

- Avoid unnecessary API calls.
- Avoid loading large images unnecessarily.
- Paginate marketplace results when appropriate.
- Use efficient MongoDB queries.
- Avoid blocking the Android main thread.
- Handle WebSocket connections efficiently.
- Avoid unnecessary Compose recompositions.
- Load marketplace content efficiently.

---

# 43. Accessibility

The UI should consider:

- Readable typography
- Sufficient contrast
- Clear button labels
- Meaningful content descriptions
- Appropriate touch targets
- Clear error messages

Accessibility should be considered during UI implementation rather than added at the end.

---

# 44. Testing Requirements

Testing is mandatory.

Every development phase must include testing.

Testing should cover:

## Android

- Gradle build
- Compilation
- UI behavior
- Navigation
- State handling
- Error states

## Backend

- API functionality
- Validation
- Authentication/authorization
- Database operations
- Error handling

## WebSocket

- Connection
- Message sending
- Message receiving
- Reconnection
- Presence
- Last seen

## AI

- Requirement extraction
- Marketplace search
- Matching
- No-result behavior
- Prevention of fabricated listings

---

# 45. Android Build Requirement

After Android implementation changes, the project must be checked with the appropriate Gradle build/check commands.

The project must not move to the next phase while the Android project has unresolved build errors.

At minimum, verify:

```text
Gradle compilation
↓
Relevant tests
↓
Build succeeds
```

---

# 46. Development Methodology

Development must happen **phase by phase**.

The required process is:

```text
PLAN
  ↓
BUILD
  ↓
TEST
  ↓
FIX
  ↓
GRADLE BUILD / VALIDATION
  ↓
REVIEW
  ↓
APPROVE
  ↓
COMMIT
  ↓
NEXT PHASE
```

---

# 47. Build → Test → Approve → Commit Rule

For every phase:

### Step 1 — Build

Implement only the functionality assigned to the current phase.

### Step 2 — Test

Test the implemented functionality.

### Step 3 — Fix

If errors exist:

```text
Error
 ↓
Investigate
 ↓
Fix
 ↓
Retest
```

Do not move forward with known broken functionality.

### Step 4 — Approve

The completed feature must be reviewed and considered stable.

### Step 5 — Commit

Create a professional Git commit.

### Step 6 — Next Phase

Only then begin the next phase.

---

# 48. Git Commit Requirements

Commit after stable features.

Commit messages must be professional and descriptive.

Preferred format:

```text
feat: implement marketplace book listings
```

Examples:

```text
feat: add Firebase authentication
feat: implement marketplace browsing
feat: add book listing creation
feat: implement book details screen
feat: add buyer seller chat
feat: implement websocket presence
feat: add AI book discovery
fix: handle failed image uploads
fix: prevent unauthorized listing updates
refactor: separate marketplace repository layer
```

Do not create meaningless commits such as:

```text
update
changes
done
test
final
```

---

# 49. Scope Control

The project should be developed according to approved requirements.

Antigravity must not:

- Add unnecessary features without approval.
- Replace the agreed technology stack without approval.
- Redesign approved UI without approval.
- Change the package name.
- Remove required functionality.
- Introduce unrelated dependencies without justification.
- Rewrite stable code unnecessarily.

If a technical decision is required that materially affects architecture, ask before making the change.

---

# 50. Ambiguity Rule

If requirements are unclear or contradictory:

```text
STOP
 ↓
Identify ambiguity
 ↓
Ask for clarification
 ↓
Wait for decision
 ↓
Continue implementation
```

Do not silently guess about important product or architecture decisions.

---

# 51. Required Documentation

The project should maintain the following documentation:

```text
docs/
├── PROJECT_REQUIREMENTS.md
├── APP_FLOW.md
├── TECH_STACK.md
├── DATABASE_SCHEMA.md
├── API_SPECIFICATION.md
├── WEBSOCKET_SPECIFICATION.md
├── AUTHENTICATION_FLOW.md
├── AI_AGENT_SPECIFICATION.md
├── UI_DESIGN_SYSTEM.md
└── DEVELOPMENT_PLAN.md
```

This document is the master product requirements document.

Other documents should contain deeper technical specifications derived from this document.

---

# 52. Initial Development Phases

The exact implementation plan may be refined before development begins, but the project should generally follow this sequence.

## Phase 1 — Project Foundation

- Android project
- Kotlin
- Jetpack Compose
- Package name
- Initial architecture
- Backend project
- Node.js
- Express.js
- MongoDB connection
- Environment configuration
- Git setup

Deliverable:

```text
Stable project foundation
```

---

## Phase 2 — Stitch Design System

Design the application's visual system and primary screens using Stitch MCP.

Define:

- Typography
- Spacing
- Components
- Colors
- Buttons
- Inputs
- Cards
- Navigation
- Marketplace UI
- Book details
- Authentication
- Listing forms
- Chat
- AI assistant

Deliverable:

```text
Approved Stitch UI
```

---

## Phase 3 — Authentication

Implement:

- Firebase Authentication
- Email/password
- Google Sign-In
- Login
- Registration
- Logout
- Authentication state

Deliverable:

```text
Stable authentication flow
```

---

## Phase 4 — Marketplace Foundation

Implement:

- Book model
- Listing API
- MongoDB integration
- Marketplace UI
- Book cards
- Book details
- Search foundation

Deliverable:

```text
Users can browse marketplace books
```

---

## Phase 5 — Selling

Implement:

- Create listing
- Image upload
- Cloudinary integration
- Book information
- Publish listing
- My Listings
- Edit listing
- Delete listing

Deliverable:

```text
Users can sell books
```

---

## Phase 6 — Search and Discovery

Implement:

- Search
- Filters
- Sorting
- Marketplace result handling
- Empty states
- Loading states
- Error states

Deliverable:

```text
Users can efficiently find books
```

---

## Phase 7 — Buyer-Seller Chat

Implement:

- WebSocket connection
- Chat creation
- One-to-one messaging
- Message persistence/handling
- Online status
- Offline status
- Last seen
- Reconnection handling

Deliverable:

```text
Buyer and seller can communicate in real time
```

---

## Phase 8 — AI Book Assistant

Implement:

- AI assistant UI
- Natural-language input
- Requirement extraction
- Marketplace search
- Matching
- Ranking
- Results
- Direct listing navigation
- No-result handling

Deliverable:

```text
AI-powered marketplace discovery
```

---

## Phase 9 — Profile and Account

Implement:

- Profile
- My Listings
- My Chats
- Account settings
- Logout
- Required account management

Deliverable:

```text
Complete user account experience
```

---

## Phase 10 — Production Hardening

Perform:

- Full testing
- Android Gradle validation
- Backend validation
- WebSocket testing
- AI testing
- Security review
- Error handling review
- UI consistency review
- Performance review
- Empty/loading/error state review

Deliverable:

```text
Production-ready BookWala MVP
```

---

# 53. Definition of Done

A feature is considered complete only when:

- It is implemented.
- UI matches the approved Stitch design.
- Functional behavior works.
- Error states are handled.
- Loading states are handled.
- Empty states are handled where relevant.
- Backend behavior works where applicable.
- Authentication/authorization is correct where applicable.
- Tests have been performed.
- Android Gradle validation passes where applicable.
- No known blocking errors remain.
- The feature has been reviewed.
- The feature is approved.
- A professional Git commit is created.

---

# 54. MVP Boundary

The first production MVP should focus on:

```text
Authentication
      +
Marketplace
      +
Buy/Sell Listings
      +
Search
      +
Book Details
      +
Buyer-Seller Chat
      +
Online/Offline/Last Seen
      +
AI Book Discovery
      +
Profile
```

The following should **not** be assumed as MVP requirements unless explicitly approved later:

- Integrated payment gateway
- Delivery/shipping system
- Order management system
- Seller ratings/reviews
- Escrow
- Book verification
- Social feed
- Notifications beyond required application behavior
- External marketplace synchronization
- Advanced recommendation systems

---

# 55. Final Product Flow

The complete intended BookWala experience is:

```text
                         BOOKWALA
                            │
                     ┌──────┴──────┐
                     │             │
                New User       Existing User
                     │             │
                     └──────┬──────┘
                            ↓
                    Authentication
                            ↓
                         Home
                            │
          ┌─────────────────┼─────────────────┐
          │                 │                 │
          ↓                 ↓                 ↓
     Marketplace         Sell Book        AI Assistant
          │                 │                 │
          ↓                 ↓                 ↓
      Search /          Create Listing    Describe Need
      Browse                 │                 │
          │                  ↓                 ↓
          ↓              Publish          AI Understands
     Book Details             │                 │
          │                  ↓                 ↓
          │              Marketplace       Search Listings
          │                                    │
          ↓                                    ↓
    I Want to Buy                         AI Results
          │                                    │
          └───────────────┬────────────────────┘
                          ↓
                    Book Details
                          ↓
                   Buyer ↔ Seller
                        Chat
                          ↓
                  Discuss / Confirm
                          ↓
                   Complete Process
```

---

# 56. Final Instruction to Antigravity

Build BookWala as a **real production-oriented second-hand book marketplace**, not as a demonstration application.

Use:

```text
Android
Kotlin + Jetpack Compose

Backend
Node.js + Express.js

Authentication
Firebase Authentication
Email/Password + Google

Database
MongoDB

Real-Time
WebSocket

Images
Cloudinary

Hosting
Render
```

Use Stitch MCP for the UI design and implement the approved Stitch designs faithfully in Jetpack Compose.

Follow the development lifecycle:

```text
PLAN
→ BUILD
→ TEST
→ FIX
→ GRADLE BUILD / VALIDATE
→ REVIEW
→ APPROVE
→ COMMIT
→ NEXT PHASE
```

Never knowingly carry broken functionality into the next phase.

If something is technically or functionally ambiguous and the decision would materially affect the application, stop and ask for clarification before proceeding.

The Android package name is:

```text
com.buildstack.bookwala
```

The project name is:

```text
BookWala
```