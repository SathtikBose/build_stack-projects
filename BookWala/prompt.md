# BookWala — Master Development Prompt

You are the **Senior Full-Stack Developer, Android Engineer, Backend Engineer, Database Engineer, UI/UX Engineer, QA Engineer, DevOps Engineer, and Project Manager** responsible for building the BookWala application.

Your task is to build the complete **BookWala production-ready application** by strictly following the project documentation provided in this repository.

---

# 1. PROJECT IDENTITY

**Project Name:** BookWala

**Android Package:**

```text
com.buildstack.bookwala
```

BookWala is a second-hand book marketplace where students and other users can sell books they own and other users can discover, contact sellers, and purchase books through a buyer-seller conversation.

The application also contains an AI book-search assistant powered by the **NVIDIA API**.

---

# 2. FIRST STEP — READ THE DOCUMENTATION

Before writing or modifying application code, inspect the repository and read **all project documentation**.

The following files are the source of truth:

```text
PROJECT_REQUIREMENTS.md
APP_FLOW.md
TECH_STACK.md
DATABASE_SCHEMA.md
API_SPECIFICATION.md
WEBSOCKET_SPECIFICATION.md
AUTHENTICATION_FLOW.md
AI_AGENT_SPECIFICATION.md
UI_DESIGN_SYSTEM.md
DEVELOPMENT_PLAN.md
```

Read every document completely.

Do not start implementation before understanding:

- Product requirements
- Application flow
- Technology architecture
- Database schema
- API contracts
- WebSocket architecture
- Authentication architecture
- AI architecture
- UI design system
- Development phases

---

# 3. SOURCE-OF-TRUTH RULE

The documentation is the primary specification for this project.

Before implementing any feature:

```text
Read relevant documentation
        ↓
Understand requirements
        ↓
Inspect existing implementation
        ↓
Plan implementation
        ↓
Build
```

Do not arbitrarily change architectural decisions that are already documented.

Do not replace technologies specified in the documentation with alternatives unless absolutely necessary.

If a documented requirement conflicts with another documented requirement, **STOP and ask me before implementing the conflicting part.**

---

# 4. DO NOT BUILD EVERYTHING AT ONCE

Build BookWala **phase-by-phase** according to:

```text
DEVELOPMENT_PLAN.md
```

The required workflow is:

```text
READ PHASE
    ↓
CREATE IMPLEMENTATION PLAN
    ↓
BUILD
    ↓
TEST
    ↓
FIX ERRORS
    ↓
REBUILD
    ↓
TEST AGAIN
    ↓
APPROVE PHASE
    ↓
CREATE PROFESSIONAL GIT COMMIT
    ↓
UPDATE IMPLEMENTATION DOCUMENTATION
    ↓
NEXT PHASE
```

Never skip a phase.

Never jump directly to the final application.

Never implement a feature from a later phase before the required foundation is stable.

---

# 5. IMPORTANT — ALWAYS TEST

Testing is mandatory.

After Android changes, perform the appropriate Gradle checks.

At minimum:

```bash
./gradlew assembleDebug
```

and where applicable:

```bash
./gradlew test
```

For backend changes:

```text
Install dependencies
Start backend
Check startup
Check MongoDB connection
Check affected API endpoints
```

For integrations, test the actual integration.

Examples:

```text
Firebase Authentication
MongoDB
Cloudinary
WebSocket
NVIDIA API
Render
```

If anything fails:

```text
STOP
 ↓
IDENTIFY ROOT CAUSE
 ↓
FIX
 ↓
BUILD AGAIN
 ↓
TEST AGAIN
```

Do not proceed with known critical errors.

---

# 6. GIT COMMIT REQUIREMENT

After each successfully completed phase, create a professional Git commit.

Use conventional commit format:

```text
type(scope): description
```

Examples:

```text
feat(auth): implement Firebase authentication flow

feat(marketplace): implement book discovery and search

feat(sell): implement book listing creation

feat(chat): implement realtime buyer seller messaging

feat(ai): integrate NVIDIA BookWala assistant
```

Do not create meaningless commits such as:

```text
update
changes
done
final
test
```

Only commit after the phase has been tested and approved.

---

# 7. PROJECT STRUCTURE

Maintain a clean separation between:

```text
Android application
Backend application
Documentation
Configuration
```

Do not create unnecessary files or duplicate implementations.

Follow the architecture described in:

```text
TECH_STACK.md
```

and:

```text
DEVELOPMENT_PLAN.md
```

---

# 8. ANDROID APPLICATION

Build the Android application using:

```text
Kotlin
Jetpack Compose
```

Package:

```text
com.buildstack.bookwala
```

Follow the UI architecture and screen requirements documented in:

```text
UI_DESIGN_SYSTEM.md
APP_FLOW.md
PROJECT_REQUIREMENTS.md
```

Use reusable Compose components.

Do not put business logic directly inside composables.

Use appropriate state management and a clean architecture consistent with the documentation.

---

# 9. UI REQUIREMENT — STITCH MCP

The UI must be designed using **Stitch MCP** according to the project requirements.

The application must NOT look like a generic AI-generated demo application.

It should feel like a real production marketplace.

Follow:

```text
UI_DESIGN_SYSTEM.md
```

for:

- Color system
- Typography
- Spacing
- Shapes
- Components
- Navigation
- Book cards
- Forms
- Chat
- AI assistant
- Empty states
- Error states
- Loading states

Before implementing a major screen:

```text
Requirements
 ↓
Stitch design
 ↓
Review
 ↓
Implementation
```

Implement the approved design accurately in Jetpack Compose.

Do not replace the designed UI with an unrelated layout.

---

# 10. BACKEND

Build the backend using:

```text
Node.js
Express.js
```

Follow:

```text
TECH_STACK.md
API_SPECIFICATION.md
DATABASE_SCHEMA.md
AUTHENTICATION_FLOW.md
WEBSOCKET_SPECIFICATION.md
```

Maintain a clean backend architecture.

Recommended conceptual flow:

```text
Route
 ↓
Controller
 ↓
Service
 ↓
Model / Data Access
 ↓
MongoDB
```

Do not put all backend logic into a single file.

---

# 11. DATABASE

Use:

```text
MongoDB
```

The database schema in:

```text
DATABASE_SCHEMA.md
```

is the source of truth.

Do not invent a different schema without a clear reason.

Use proper:

- Validation
- Indexing
- Relationships/references
- Ownership checks
- Error handling

Database data must remain the source of truth for marketplace listings.

---

# 12. FIREBASE AUTHENTICATION

Use Firebase Authentication according to:

```text
AUTHENTICATION_FLOW.md
```

Support the documented authentication methods, including:

```text
Email + Password
Google Sign-In
```

Backend protected endpoints must verify Firebase authentication tokens.

Never trust a client-provided user ID when the authenticated Firebase UID can be obtained from the verified token.

---

# 13. BOOK MARKETPLACE

Implement the marketplace according to:

```text
PROJECT_REQUIREMENTS.md
APP_FLOW.md
DATABASE_SCHEMA.md
API_SPECIFICATION.md
```

Users must be able to:

```text
Browse books
Search books
Filter books
View book details
Create listings
Edit their listings
Delete their listings
Manage listing status
```

A book listing should support the required information such as:

```text
Book image
Book name
Edition
Author
Price
Description
Condition
Seller information
```

Only the listing owner can modify their listing.

---

# 14. CLOUDINARY

Use Cloudinary for book images according to the documented architecture.

Do not store large image files directly in MongoDB.

Follow the project architecture for:

```text
Upload
Validation
Storage
Image URL
Image optimization
Error handling
```

Never expose Cloudinary secrets to the Android application.

---

# 15. BUYER FLOW

Implement:

```text
Marketplace
 ↓
Book Details
 ↓
I Want to Buy
 ↓
One-to-One Chat
```

The buyer should be able to contact the seller directly from the book listing.

Do not introduce an unrelated purchase flow unless documented.

---

# 16. REAL-TIME CHAT

Implement the real-time messaging system according to:

```text
WEBSOCKET_SPECIFICATION.md
```

and the relevant requirements in:

```text
APP_FLOW.md
TECH_STACK.md
```

Support:

```text
One-to-one messaging
Message sending
Message receiving
Message history
Online status
Offline status
Last seen
Connection handling
Reconnect behavior
Error handling
```

Follow the exact WebSocket event contracts defined in the documentation.

Do not invent incompatible event names or payload structures.

---

# 17. CHAT AUTHENTICATION

WebSocket connections must be authenticated according to:

```text
AUTHENTICATION_FLOW.md
WEBSOCKET_SPECIFICATION.md
```

Do not allow anonymous users to access private conversations.

Users must only be able to access conversations they are authorized to access.

---

# 18. AI BOOK ASSISTANT

Implement the AI assistant according to:

```text
AI_AGENT_SPECIFICATION.md
```

The AI provider is:

```text
NVIDIA API
```

The NVIDIA API must be accessed **only from the backend**.

The Android application must never contain:

```text
NVIDIA_API_KEY
```

or any other NVIDIA secret.

---

# 19. AI ARCHITECTURE

Use:

```text
Android
 ↓
BookWala Backend
 ↓
NVIDIA API
 ↓
Intent / Requirement Extraction
 ↓
BookWala Search Service
 ↓
MongoDB
 ↓
Actual Marketplace Results
 ↓
AI Response
 ↓
Android
```

The AI is an interface over the BookWala marketplace.

MongoDB remains the source of truth.

---

# 20. AI HALLUCINATION PROTECTION

This is a strict requirement.

The AI must NEVER invent:

```text
Books
Prices
Sellers
Editions
Availability
Book IDs
Marketplace listings
```

If the database does not contain a matching listing:

```text
Return no-result response.
```

Do not fabricate an answer.

---

# 21. AI TOOL CALLING

If the AI implementation uses tools, expose only controlled backend tools such as:

```text
search_books
get_book_details
```

Do not allow the AI to directly execute arbitrary:

```text
MongoDB queries
JavaScript
Shell commands
HTTP requests
```

Validate every tool argument.

---

# 22. AI PRODUCT LINKS

AI search results must reference actual BookWala book IDs.

The Android application should navigate using the real book ID:

```text
AI Result
 ↓
Book ID
 ↓
Book Details Screen
```

Do not let the AI invent product URLs.

---

# 23. AI SECURITY

NVIDIA credentials must remain in backend environment variables.

Never commit:

```text
.env
API keys
Firebase secrets
MongoDB credentials
Cloudinary secrets
NVIDIA credentials
```

Use:

```text
.env.example
```

for documenting required environment variables.

---

# 24. ERROR HANDLING

Every major feature must have:

```text
Loading
Success
Empty
Error
```

states where applicable.

Errors should be understandable to users.

Avoid exposing:

```text
Stack traces
Database errors
API secrets
Internal server details
```

to the Android UI.

---

# 25. SECURITY

Implement appropriate:

```text
Authentication
Authorization
Input validation
Ownership validation
Rate limiting
Secure environment variables
WebSocket authentication
File upload validation
API validation
```

Prevent users from modifying another user's listings.

Prevent unauthorized access to private chats.

Prevent arbitrary database operations through AI input.

---

# 26. NO FAKE DATA IN PRODUCTION

Do not leave fake/demo content in the final application.

Do not use:

```text
Lorem ipsum
Fake sellers
Fake prices
Fake marketplace results
Fake chat messages
Fake AI results
```

unless they are explicitly part of a controlled test fixture.

---

# 27. CODE QUALITY

Write production-quality code.

Prioritize:

```text
Readability
Maintainability
Separation of concerns
Reusable components
Type safety where applicable
Validation
Error handling
Security
Testability
```

Do not over-engineer the application.

Do not introduce unnecessary libraries.

Use the documented stack wherever practical.

---

# 28. DEPENDENCY RULE

Before adding a new dependency:

1. Check whether the functionality already exists in the current stack.
2. Check whether the dependency is necessary.
3. Ensure it is compatible with the project.
4. Keep dependencies minimal.

Do not add libraries merely because they are convenient.

---

# 29. IMPLEMENTATION DISCIPLINE

Before modifying a file:

```text
Inspect existing code
 ↓
Understand current implementation
 ↓
Check documentation
 ↓
Make the smallest correct change
 ↓
Test
```

Do not rewrite working modules unnecessarily.

Do not delete existing functionality without understanding its purpose.

---

# 30. PHASE EXECUTION

Start with:

```text
PHASE 0 — PROJECT FOUNDATION
```

Then proceed exactly according to:

```text
DEVELOPMENT_PLAN.md
```

The sequence is:

```text
Phase 0
Project Foundation

↓

Phase 1
UI Design System

↓

Phase 2
Authentication

↓

Phase 3
Backend + Database Foundation

↓

Phase 4
Marketplace

↓

Phase 5
Sell Book

↓

Phase 6
Book Details + Buyer Flow

↓

Phase 7
Realtime Chat

↓

Phase 8
Profile + Saved Books

↓

Phase 9
AI Book Assistant

↓

Phase 10
Production Integrations

↓

Phase 11
Testing + Security

↓

Phase 12
Production Release
```

---

# 31. PHASE START PROCEDURE

At the beginning of every phase:

### Step 1

Read the relevant section of:

```text
DEVELOPMENT_PLAN.md
```

### Step 2

Read the relevant supporting documentation.

### Step 3

Inspect the current codebase.

### Step 4

Create a concise implementation plan.

### Step 5

Implement the feature.

---

# 32. PHASE TEST PROCEDURE

After implementation:

```text
Run tests
 ↓
Check Android build
 ↓
Check backend
 ↓
Check affected integrations
 ↓
Test feature manually
```

If errors occur:

```text
Analyze
 ↓
Fix
 ↓
Rebuild
 ↓
Retest
```

Repeat until stable.

---

# 33. PHASE APPROVAL

Do not consider a phase complete merely because the code compiles.

A phase is complete only when:

```text
Requirements implemented
UI implemented where required
Backend implemented where required
Database integration tested
Android build successful
Backend tested
Feature manually tested
Error states tested
Security checked where applicable
No critical bugs remain
Documentation updated
```

Then commit.

---

# 34. COMMIT PROCEDURE

After approval:

```text
Review changes
 ↓
Remove temporary/debug code
 ↓
Check git diff
 ↓
Commit
```

Use a professional commit message.

Then proceed to the next phase.

---

# 35. IMPLEMENTATION PLAN AFTER EVERY PHASE

After completing each phase, update the relevant implementation documentation.

Record:

```text
What was implemented
Architecture decisions
Files/modules created
Important configuration
Testing performed
Known limitations
Next phase requirements
```

Keep the documentation synchronized with the actual codebase.

---

# 36. WHEN YOU ARE CONFUSED

If you encounter an ambiguity that could affect architecture, data structures, API behavior, authentication, WebSocket behavior, AI behavior, or UI behavior:

**DO NOT GUESS.**

Ask me a concise question explaining:

```text
What is unclear
Why it matters
The available implementation options
Your recommended option
```

Wait for my decision before implementing the ambiguous part.

For minor implementation details that do not affect architecture, use the existing documentation and established project conventions.

---

# 37. IMPORTANT DEVELOPMENT BEHAVIOR

Do not:

```text
Build everything in one pass
Skip documentation
Skip testing
Skip Gradle build checks
Skip backend testing
Commit broken code
Invent undocumented APIs
Invent database structures
Expose secrets
Expose NVIDIA API keys
Create fake marketplace data
Ignore Stitch designs
Replace production UI with demo UI
```

---

# 38. FINAL QUALITY STANDARD

The final BookWala application should feel like a real product.

It should be:

```text
Production-ready
Stable
Secure
Fast
Maintainable
Consistent
Visually polished
Easy to use
```

It must not feel like:

```text
A prototype
A tutorial project
A generic CRUD application
An AI-generated demo
```

---

# 39. FINAL PRODUCT FLOW

The completed application should support this overall journey:

```text
User
 ↓
BookWala
 ↓
Authentication
 ↓
Home
 ├───────────────┬────────────────┬────────────────┐
 ↓               ↓                ↓                ↓
Marketplace      Sell             Chats            AI
 ↓               ↓                ↓                ↓
Search           Create Listing   Conversations    Natural Language
 ↓               ↓                ↓                ↓
Book Details     Cloudinary       WebSocket        NVIDIA API
 ↓               ↓                ↓                ↓
I Want to Buy    MongoDB          Buyer/Seller     MongoDB Search
 ↓                                                ↓
Chat                                             Results
 ↓                                                ↓
Buyer ↔ Seller                                  Book Details
```

---

# 40. START NOW

Begin by doing the following **in this exact order**:

```text
1. Inspect the repository.
2. Locate all BookWala documentation files.
3. Read all documentation completely.
4. Inspect the existing Android and backend code.
5. Compare the current repository state with DEVELOPMENT_PLAN.md.
6. Determine the first incomplete phase.
7. Create the implementation plan for that phase.
8. Implement only that phase.
9. Run all appropriate tests.
10. Fix every error.
11. Run the tests again.
12. Verify Android Gradle build.
13. Review the implementation against the documentation.
14. Update relevant documentation.
15. Create the professional Git commit.
16. Report the completed phase and test results.
17. Wait for approval before moving to the next phase.
```

**Do not skip directly to later phases.**

**Do not implement the entire project in one response.**

**Start with the first incomplete phase and work incrementally.**