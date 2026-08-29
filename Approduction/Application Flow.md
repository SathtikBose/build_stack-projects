# Application Flow

# AP Production

**Project Name:** AP Production  
**Package Name:** `com.buildstack.approduction`

---

# 1. Complete Application Flow

```mermaid
flowchart TD

    A[User Opens AP Production] --> B[Splash Screen]

    B --> C{Valid Login Session Exists?}

    C -->|Yes| D[Dashboard]
    C -->|No| E[Login Screen]

    E --> F[Enter Email and Password]
    F --> G[Press Login]

    G --> H{Credentials Valid?}

    H -->|No| I[Show Login Error]
    I --> E

    H -->|Yes| J[Save Secure Session]
    J --> D

    D --> K[Add Previous Day Production]
    D --> L[View Daily Data]
    D --> M[Generate Report]
    D --> N[Logout]

    N --> O[Clear Local Session]
    O --> E
```

---

# 2. Authentication Flow

```mermaid
flowchart TD

    A[App Launch] --> B[Check Secure Session]

    B --> C{Session Exists?}

    C -->|No| D[Show Login Screen]

    C -->|Yes| E[Validate Session]

    E --> F{Valid?}

    F -->|Yes| G[Open Dashboard]

    F -->|No| H[Clear Invalid Session]
    H --> D

    D --> I[User Enters Email]
    I --> J[User Enters Password]

    J --> K[Validate Local Input]

    K --> L{Valid Input?}

    L -->|No| M[Show Validation Error]
    M --> D

    L -->|Yes| N[Send Login Request]

    N --> O{Login Successful?}

    O -->|No| P[Show Invalid Credentials]
    P --> D

    O -->|Yes| Q[Save JWT Securely]
    Q --> G
```

---

# 3. Dashboard Flow

```mermaid
flowchart TD

    A[Dashboard]

    A --> B[Add Previous Day Production]

    A --> C[View Daily Data]

    A --> D[Generate Report]

    A --> E[Logout]
```

---

# 4. Add Production Data Flow

```mermaid
flowchart TD

    A[Tap Add Previous Day Production]

    A --> B[Set Production Date]

    B --> C[Current Date - 1]

    C --> D[Check Existing Draft]

    D --> E{Draft Exists?}

    E -->|Yes| F[Show Resume Draft Dialog]

    F --> G[Continue Draft]
    F --> H[Discard Draft]

    G --> I[Production Entry Flow]

    H --> J[Clear Draft]
    J --> I

    E -->|No| I

    I --> K[Select Pressshop]

    K --> L{Pressshop Selected?}

    L -->|No| K

    L -->|Yes| M[Show Machines]

    M --> N[Select Machine]

    N --> O[Enter Total Working Hours]

    O --> P[Enter Machine Working Hours]

    P --> Q[Enter Stroke]

    Q --> R[Calculate Idle Time]

    R --> S[Select Idle Reason Type]

    S --> T{Idle Reason Type}

    T -->|None| U[No Idle Reason Entry Required]

    T -->|Man Power Shortage| V[Enter Idle Hours and Details]

    T -->|Breakdown| V

    T -->|No Work| V

    U --> W[Save Form Draft]

    V --> W

    W --> X[Press Save Data]

    X --> Y[Validate Form]

    Y --> Z{Form Valid?}

    Z -->|No| AA[Show Field Validation Errors]
    AA --> I

    Z -->|Yes| AB[Send Production Data to Backend]

    AB --> AC{Save Successful?}

    AC -->|No| AD[Keep Draft Locally]
    AD --> AE[Show Save Error]

    AE --> AF[Retry Save]

    AF --> AB

    AC -->|Yes| AG[Clear Local Draft]

    AG --> AH[Show Toast: Data Saved Successfully]

    AH --> AI[Return to Dashboard]
```

---

# 5. Pressshop and Machine Flow

```mermaid
flowchart TD

    A[Select Pressshop]

    A --> B{Selection}

    B -->|Pressshop - 1| C[Load Pressshop - 1 Machines]

    B -->|Pressshop - 2| D[Load Pressshop - 2 Machines]

    C --> E[Display Machines in Fixed Sequence]

    D --> E

    E --> F[User Selects Machine]

    F --> G[Confirm Machine Selection]
```

---

# 6. Machine Sequence Flow

```mermaid
flowchart LR

    A[Pressshop Selected]

    A --> B[Fetch Machine List]

    B --> C[Sort by Sequence Field]

    C --> D[Display Machine List]

    D --> E[User Selects Machine]
```

The application must always use the predefined sequence value.

Machines must never be automatically reordered alphabetically.

---

# 7. Idle Time Flow

```mermaid
flowchart TD

    A[Enter Total Working Hours]

    A --> B[Enter Machine Working Hours]

    B --> C[Calculate Idle Time]

    C --> D[Idle Time = Total Working Hours - Machine Working Hours]

    D --> E{Idle Time > 0?}

    E -->|No| F[Idle Reason = None]

    E -->|Yes| G[Select Idle Reason Type]

    G --> H{Reason Type}

    H -->|Man Power Shortage| I[Enter Idle Hours]

    H -->|Breakdown| J[Enter Idle Hours]

    H -->|No Work| K[Enter Idle Hours]

    I --> L[Enter Reason Details]

    J --> L

    K --> L
```

---

# 8. Save Production Data Flow

```mermaid
sequenceDiagram

    actor User

    participant App as Android App

    participant Local as Local Storage

    participant API as Backend API

    participant DB as MongoDB

    User->>App: Enter Production Data

    App->>Local: Save Current Draft

    User->>App: Press Save

    App->>App: Validate Data

    alt Invalid Data

        App-->>User: Show Validation Error

    else Valid Data

        App->>API: Send Production Record

        API->>API: Validate Token

        API->>API: Validate Request Data

        API->>DB: Check Existing Record

        alt Existing Record

            DB-->>API: Existing Record

            API->>DB: Update Record

        else New Record

            API->>DB: Create Record

        end

        DB-->>API: Save Success

        API-->>App: Success Response

        App->>Local: Clear Draft

        App-->>User: Toast Data Saved Successfully

    end
```

---

# 9. Daily Data Viewing Flow

```mermaid
flowchart TD

    A[Tap View Daily Data]

    A --> B[Select Date]

    B --> C[Select Pressshop]

    C --> D[Load Machines]

    D --> E[Select Machine]

    E --> F[Tap View Data]

    F --> G[Request Data from Backend]

    G --> H{Data Found?}

    H -->|Yes| I[Display Production Details]

    H -->|No| J[Show No Data Found]

    J --> K[Add Production Data]

    K --> L[Production Entry]
```

---

# 10. Report Generation Flow

```mermaid
flowchart TD

    A[Tap Generate Report]

    A --> B[Select From Date]

    B --> C[Select To Date]

    C --> D{Date Range Valid?}

    D -->|No| E[Show Date Error]

    E --> B

    D -->|Yes| F[Select Pressshop]

    F --> G[Select Machine or All Machines]

    G --> H[Tap Generate Report]

    H --> I[Send Report Request]

    I --> J[Backend Fetches Production Records]

    J --> K[Group Records by Machine]

    K --> L[Calculate Totals]

    L --> M[Display Report]
```

---

# 11. Report Calculation Flow

```mermaid
flowchart TD

    A[Production Records]

    A --> B[Group by Machine]

    B --> C[Calculate Total Working Hours]

    B --> D[Calculate Machine Working Hours]

    B --> E[Calculate Total Stroke]

    C --> F[Calculate Idle Time]

    D --> F

    F --> G[Total Working Hours - Machine Working Hours]

    B --> H[Calculate Man Power Shortage Hours]

    B --> I[Calculate Breakdown Hours]

    B --> J[Calculate No Work Hours]

    C --> K[Final Machine Report]

    D --> K

    E --> K

    G --> K

    H --> K

    I --> K

    J --> K
```

---

# 12. CSV Export Flow

```mermaid
flowchart TD

    A[Report Generated]

    A --> B[Tap Download CSV]

    A --> C[Tap Share CSV]

    B --> D[Generate CSV]

    C --> D

    D --> E[Backend Returns CSV File]

    E --> F{User Action}

    F -->|Download| G[Save CSV Locally]

    F -->|Share| H[Open Android Share Sheet]

    G --> I[Success Message]

    H --> I
```

---

# 13. Draft Persistence Flow

```mermaid
flowchart TD

    A[User Opens Production Entry]

    A --> B[Check Local Draft]

    B --> C{Draft Available?}

    C -->|No| D[Create New Entry]

    C -->|Yes| E[Show Draft Recovery Dialog]

    E --> F[Continue Draft]

    E --> G[Discard Draft]

    F --> H[Restore All Form Values]

    G --> I[Delete Draft]

    H --> J[Continue Data Entry]

    I --> D

    J --> K[Auto Save Changes]

    D --> K
```

---

# 14. Logout Flow

```mermaid
flowchart TD

    A[User Opens Logout]

    A --> B[Confirm Logout]

    B --> C{Confirmed?}

    C -->|No| D[Return to Dashboard]

    C -->|Yes| E[Clear JWT]

    E --> F[Clear Session Data]

    F --> G[Clear Authentication State]

    G --> H[Navigate to Login]
```

---

# 15. Error Handling Flow

```mermaid
flowchart TD

    A[User Action]

    A --> B{Internet Available?}

    B -->|No| C[Show Network Error]

    C --> D[Keep Current Data]

    D --> E[Retry]

    B -->|Yes| F[Send Request]

    F --> G{Server Error?}

    G -->|Yes| H[Show Friendly Error]

    H --> D

    G -->|No| I[Process Response]

    I --> J{Success?}

    J -->|Yes| K[Show Success Feedback]

    J -->|No| L[Show Validation or API Error]
```

---

# 16. Complete User Journey

```mermaid
journey
    title AP Production User Journey

    section Login
      Open Application: 5: User
      Check Session: 5: App
      Login if Required: 4: User

    section Dashboard
      View Main Options: 5: User
      Select Add Production: 5: User

    section Production Entry
      Confirm Previous Day: 5: User
      Select Pressshop: 5: User
      Select Machine: 5: User
      Enter Working Hours: 4: User
      Enter Stroke: 4: User
      Add Idle Reason: 4: User

    section Save
      Review Data: 4: User
      Save Production Data: 5: User
      Receive Success Toast: 5: User

    section Reporting
      Select Date Range: 4: User
      Generate Report: 5: User
      Export CSV: 5: User
```

---

# 17. Application Navigation Summary

```mermaid
flowchart LR

    A[Login]

    A --> B[Dashboard]

    B --> C[Add Previous Day Production]

    B --> D[View Daily Data]

    B --> E[Generate Report]

    C --> F[Pressshop]

    F --> G[Machine]

    G --> H[Production Form]

    H --> B

    D --> I[Data Results]

    I --> B

    E --> J[Report Results]

    J --> K[Download CSV]

    J --> L[Share CSV]

    J --> B
```

---

# 18. Primary Application Rules

```text
1. Default production date must be Current Date - 1.

2. Users must select a Pressshop before selecting a machine.

3. Machine order must remain exactly as defined.

4. Machine Working Hours cannot exceed Total Working Hours.

5. Idle Time must be calculated automatically.

6. Idle Time = Total Working Hours - Machine Working Hours.

7. Draft data must persist if the user leaves the application.

8. Drafts must only be cleared after successful save or user confirmation.

9. Production data must be available for any saved date.

10. Reports must support a From Date and To Date.

11. CSV reports must contain machine-wise totals.

12. Users must authenticate before accessing protected application features.

13. Valid login sessions must support automatic login.

14. Logout must clear the stored authentication session.
```

---

# 19. Final Application Flow

```mermaid
flowchart TD

    A[Launch App]

    A --> B{Logged In?}

    B -->|No| C[Login]

    B -->|Yes| D[Dashboard]

    C --> D

    D --> E[Add Production]

    E --> F[Select Pressshop]

    F --> G[Select Machine]

    G --> H[Enter Production Details]

    H --> I[Save Draft]

    I --> J[Save to Backend]

    J --> K{Success?}

    K -->|Yes| L[Toast Success]

    K -->|No| M[Keep Draft]

    L --> D

    M --> H

    D --> N[View Daily Data]

    N --> O[Select Date and Machine]

    O --> P[View Production Data]

    P --> D

    D --> Q[Generate Report]

    Q --> R[Select Date Range]

    R --> S[Generate Machine Summary]

    S --> T[Download CSV]

    S --> U[Share CSV]

    T --> D

    U --> D
```