# Project Development Instructions

# AP Production

These instructions are mandatory during the development of AP Production.

---

# 1. Project Identity

**Project Name:** AP Production

**Android Package Name:**

```text
com.buildstack.approduction
```

The project must use a monorepo architecture.

---

# 2. Technology Stack

## Mobile Application

```text
Android
Kotlin
Jetpack Compose
MVVM
Hilt
Retrofit
Room
DataStore
```

## Backend

```text
Node.js
Express.js
MongoDB
Mongoose
JWT
bcrypt
```

## Deployment

```text
Render
MongoDB Atlas
```

---

# 3. Mandatory Development Process

Every stable implementation must follow:

```text
Build
↓
Test
↓
Fix
↓
Test Again
↓
Git Commit
```

Never skip testing before committing stable work.

---

# 4. Android Build Instruction

The Android application must always be built using Gradle.

Primary build command:

```bash
./gradlew build
```

Clean build:

```bash
./gradlew clean
./gradlew build
```

Before committing Android changes:

```text
1. Build the app.
2. Check build errors.
3. Fix errors.
4. Build again.
5. Run tests.
6. Manually test the feature.
7. Commit stable changes.
```

---

# 5. Backend Build Instruction

Before committing backend changes:

```text
1. Install dependencies.
2. Run backend.
3. Test affected APIs.
4. Run automated tests.
5. Fix errors.
6. Test again.
7. Commit stable changes.
```

Do not assume the backend works without testing the actual API behavior.

---

# 6. Phase-Wise Development

The project must be developed in phases.

Recommended phase order:

```text
Phase 1 — Monorepo Setup

Phase 2 — Backend Foundation

Phase 3 — Authentication

Phase 4 — Pressshop and Machine Master Data

Phase 5 — Production Data API

Phase 6 — Report and CSV API

Phase 7 — Android Foundation

Phase 8 — Android Authentication

Phase 9 — Production Entry

Phase 10 — Daily Data Viewer

Phase 11 — Reports

Phase 12 — Integration Testing

Phase 13 — Deployment
```

Do not start a later phase when the previous phase is unstable.

---

# 7. Missing Information Rule

Before implementing functionality that depends on missing information:

```text
Ask the user for clarification.
```

Do not invent business rules.

Examples of missing information that may require clarification:

- Multiple idle reasons for one day.
- Whether existing records should be updated automatically.
- Maximum working hours.
- User roles and permissions.
- Exact report requirements.
- Whether data can be deleted.
- Data retention requirements.

If the information is not required for the current implementation, use the existing documented requirements.

---

# 8. UI/UX Instructions

The application must be designed as a professional production management application.

It must not look like:

```text
An AI-generated demo app
A template application
A generic dashboard
A prototype
```

The UI must be:

```text
Professional
Simple
Clean
Consistent
Easy to use
Practical
Accessible
```

---

# 9. Target User Usability

Assume that some users may not know how to use mobile applications well.

Therefore:

- Use clear labels.
- Use large buttons.
- Avoid hidden actions.
- Avoid unnecessary navigation.
- Use readable font sizes.
- Show clear validation messages.
- Use simple language.
- Keep the number of steps low.

---

# 10. Machine Sequence Rule

The following sequence is mandatory.

The order must never be changed.

The machine list must never be alphabetically sorted.

## Pressshop - 1

```text
1. Break Press 300 Ton
2. Break Press 400 Ton
3. 1200 Ton
4. 1000 Ton F
5. 3000 Ton
6. 1100 Ton
7. 800 Ton
8. 400 Ton
9. 600 Ton
10. 315 Ton
11. 500 Ton
12. 1000 Ton USI
13. Schuller
14. Muller
15. 1250 Ton
16. 2000 Ton
17. 350 Ton
```

## Pressshop - 2

```text
1. 1000 Ton ISG
2. 400 Ton
3. 1500 Ton
4. 1000 Ton 2
5. 1000 Ton 1
6. 800 Ton 1
7. 800 Ton 2
8. 300 Ton 1
9. 300 Ton 2
```

The database must store an explicit numeric sequence field.

Example:

```text
Pressshop - 1
Break Press 300 Ton → sequence: 1
Break Press 400 Ton → sequence: 2
```

All APIs must sort by:

```text
sequence ASC
```

---

# 11. Production Date Rule

The default production date must always be:

```text
Current Date - 1
```

Example:

```text
Current Date: 28 August 2026
Default Production Date: 27 August 2026
```

The implementation must use the device's local date.

---

# 12. Production Data Validation

The following rules are mandatory:

```text
Total Working Hours >= 0
```

```text
Machine Working Hours >= 0
```

```text
Machine Working Hours <= Total Working Hours
```

```text
Stroke >= 0
```

```text
Idle Hours >= 0
```

The backend must also validate all fields.

Frontend validation alone is not sufficient.

---

# 13. Idle Time Rule

Idle time must be calculated automatically:

```text
Idle Time = Total Working Hours - Machine Working Hours
```

The user should not manually enter the calculated total idle time.

---

# 14. Idle Reason Types

Only the following values are valid:

```text
NONE
MAN_POWER_SHORTAGE
BREAKDOWN
NO_WORK
```

User-facing labels:

```text
None
Man Power Shortage
Breakdown
No Work
```

Do not allow arbitrary reason types.

---

# 15. State Persistence Rule

The production entry form must preserve the user's current work.

If the user:

- Closes the application.
- Minimizes the application.
- Changes screens.
- Accidentally leaves the form.

The form data must remain available.

The application should restore the latest draft.

A draft must be removed only when:

```text
Data is successfully saved
OR
The user explicitly discards the draft
```

---

# 16. Authentication Rule

The application must provide:

```text
Email
Password
Login
Auto Login
Logout
```

Passwords must not be stored in the Android application.

Backend credentials must be stored securely using environment variables.

The `.env` file must never be committed.

---

# 17. Environment Variable Rule

Use:

```text
.env
```

for local secrets.

Use:

```text
.env.example
```

for documented variable names.

Never commit:

```text
Database passwords
JWT secrets
Real credentials
Production tokens
API keys
```

---

# 18. Backend Security Instructions

The backend must:

- Use HTTPS in production.
- Require authentication for protected routes.
- Validate JWT tokens.
- Hash passwords with bcrypt.
- Use request size limits.
- Reject unexpected request payloads.
- Validate all incoming data.
- Use rate limiting.
- Use Helmet.
- Configure CORS.
- Use centralized error handling.

---

# 19. File Upload Security Rule

The application does not require general file upload functionality.

Therefore:

```text
Do not implement file upload endpoints unless explicitly required later.
```

The backend must reject unsupported upload requests.

This prevents unwanted files from being sent to or stored on the backend.

---

# 20. API Rule

All protected API endpoints must require authentication.

API structure:

```text
Request
↓
Authentication
↓
Authorization
↓
Validation
↓
Controller
↓
Service
↓
Database
↓
Response
```

All responses must use a consistent format.

Success:

```json
{
  "success": true,
  "message": "Operation successful",
  "data": {}
}
```

Error:

```json
{
  "success": false,
  "message": "Error message",
  "errors": []
}
```

---

# 21. CSV Report Rule

CSV reports must contain:

```text
Machine
Total Working Hours
Machine Working Hours
Total Stroke
Idle Time
Man Power Shortage Hours
Breakdown Hours
No Work Hours
```

The CSV must preserve the machine sequence.

---

# 22. Report Calculation Rules

For every machine:

```text
Total Working Hours
=
SUM(totalWorkingHours)
```

```text
Machine Working Hours
=
SUM(machineWorkingHours)
```

```text
Total Stroke
=
SUM(stroke)
```

```text
Idle Time
=
SUM(totalWorkingHours - machineWorkingHours)
```

```text
Man Power Shortage Hours
=
SUM(MAN_POWER_SHORTAGE idle hours)
```

```text
Breakdown Hours
=
SUM(BREAKDOWN idle hours)
```

```text
No Work Hours
=
SUM(NO_WORK idle hours)
```

---

# 23. Duplicate Data Rule

The database must prevent multiple production records for the same:

```text
Machine
+
Production Date
```

Recommended unique constraint:

```text
machineId + productionDate
```

The expected behavior when a duplicate is found must be handled consistently according to the finalized implementation requirements.

---

# 24. Toast Notification Rule

Use Toast notifications for successful simple actions.

Examples:

```text
Data Saved Successfully
Production Data Updated Successfully
Login Successful
```

Important errors should also be visible through clear UI messages.

Do not rely only on Toast notifications for critical validation errors.

---

# 25. Logging Rule

Never log:

```text
Passwords
JWT tokens
JWT secrets
Database credentials
Password hashes
Environment variables
```

Errors must be logged safely.

---

# 26. Testing Rule

Every major feature must be tested.

Minimum test categories:

```text
Unit Testing
API Testing
Integration Testing
Manual Testing
```

Critical calculations must have automated tests.

---

# 27. Git Commit Rule

Commit only after:

```text
Build Success
+
Test Success
+
Stable Feature Verification
```

Use meaningful commit messages.

Examples:

```text
feat: add production entry form

fix: correct idle time calculation

test: add report aggregation tests

chore: configure backend environment
```

---

# 28. Deployment Rule

The backend deployment target is:

```text
Render
```

Production secrets must be configured through deployment environment variables.

Do not use development credentials in the production environment.

---

# 29. Final Verification Rule

Before declaring a phase complete:

```text
[ ] Feature implemented
[ ] Build successful
[ ] Tests passed
[ ] Errors fixed
[ ] Manual verification complete
[ ] Documentation updated if required
[ ] Stable Git commit created
```

---

# 30. Most Important Rules

```text
1. Build before committing.

2. Test every stable feature.

3. Fix all known build and test issues.

4. Test again after fixing.

5. Commit only stable builds.

6. Develop phase by phase.

7. Ask the user when important requirements are missing.

8. Never change the machine sequence.

9. Do not create a generic AI-looking UI.

10. Preserve unsaved user data.

11. Secure the backend against invalid requests.

12. Never expose secrets.

13. Use Gradle for Android builds.

14. Use the monorepo structure.
```