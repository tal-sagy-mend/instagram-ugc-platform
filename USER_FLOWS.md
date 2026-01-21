# User Flow Diagrams

Visual flow diagrams for key user journeys in the Instagram UGC Platform.

## Flow 1: Discover and Save Content

```mermaid
flowchart TD
    A[Dashboard] --> B[Click Discover Content]
    B --> C[Content Discovery Page]
    C --> D[Enter Search Query]
    D --> E[Apply Filters]
    E --> F[Browse Results Grid]
    F --> G{Click Content Item?}
    G -->|Yes| H[Content Detail Modal]
    G -->|No| F
    H --> I[Review Content & Metadata]
    I --> J[Click Save to Collection]
    J --> K{Collection Exists?}
    K -->|Yes| L[Select Collection]
    K -->|No| M[Create New Collection]
    M --> L
    L --> N[Content Saved Successfully]
    N --> O{Continue Browsing?}
    O -->|Yes| F
    O -->|No| P[Return to Dashboard]
```

## Flow 2: Request Permission

```mermaid
flowchart TD
    A[Content Library/Detail View] --> B[Select Content Item]
    B --> C[Click Request Permission]
    C --> D[Permission Request Modal]
    D --> E[Select Contact Method]
    E --> F[Choose Permission Template]
    F --> G[Set Usage Rights]
    G --> H[Set Expiration Date]
    H --> I[Review Message Preview]
    I --> J{Ready to Send?}
    J -->|Edit| E
    J -->|Send| K[Send Request]
    K --> L[Request Status: Pending]
    L --> M[Notification Sent to Creator]
    M --> N[Track in Permission Dashboard]
    N --> O{Creator Responds?}
    O -->|Approves| P[Status: Approved]
    O -->|Denies| Q[Status: Denied]
    O -->|No Response| R[Send Reminder]
    R --> O
    P --> S[Content Available for Use]
    Q --> T[Content Cannot Be Used]
```

## Flow 3: Export Content

```mermaid
flowchart TD
    A[Content Library/Collection] --> B[Select Content Items]
    B --> C{Multiple Items?}
    C -->|Yes| D[Bulk Select]
    C -->|No| E[Single Select]
    D --> F[Click Export]
    E --> F
    F --> G[Export Modal]
    G --> H[Choose Export Format]
    H --> I[Select Metadata to Include]
    I --> J[Choose Delivery Method]
    J --> K{Delivery Type?}
    K -->|Download| L[Process & Create ZIP]
    K -->|Email| M[Process & Send Email]
    K -->|Cloud Storage| N[Process & Upload]
    L --> O[Show Progress Bar]
    M --> O
    N --> O
    O --> P{Processing Complete?}
    P -->|No| O
    P -->|Yes| Q[Export Complete]
    Q --> R[Success Notification]
    R --> S[Download/Email/Cloud Link]
```

## Flow 4: Content Organization

```mermaid
flowchart TD
    A[Content Library] --> B[View All Content]
    B --> C{Organize By?}
    C -->|Collection| D[Filter by Collection]
    C -->|Tag| E[Filter by Tag]
    C -->|Permission Status| F[Filter by Status]
    C -->|Date| G[Filter by Date Range]
    D --> H[View Collection Contents]
    E --> H
    F --> H
    G --> H
    H --> I{Add to Collection?}
    I -->|Yes| J[Select Collection]
    I -->|No| K{Add Tags?}
    J --> L[Content Added]
    K -->|Yes| M[Add Tags]
    K -->|No| H
    M --> L
    L --> H
```

## Flow 5: Permission Management Workflow

```mermaid
flowchart TD
    A[Permission Dashboard] --> B{Filter By?}
    B -->|Status| C[View by Status]
    B -->|Creator| D[View by Creator]
    B -->|Date| E[View by Date]
    C --> F[View Permission List]
    D --> F
    E --> F
    F --> G{Action Needed?}
    G -->|Pending| H[Send Reminder]
    G -->|Approved| I[View Details]
    G -->|Expiring Soon| J[Renew Permission]
    G -->|Expired| K[Request Renewal]
    H --> L[Reminder Sent]
    I --> M[View Usage History]
    J --> N[Extend Expiration]
    K --> O[Send New Request]
    L --> F
    M --> F
    N --> F
    O --> F
```

## Flow 6: User Onboarding

```mermaid
flowchart TD
    A[Sign Up/Login] --> B[Welcome Screen]
    B --> C[Connect Instagram Account]
    C --> D[OAuth Flow]
    D --> E{Connected?}
    E -->|No| C
    E -->|Yes| F[Account Connected]
    F --> G[Guided Tour]
    G --> H[Step 1: Discover Content]
    H --> I[Step 2: Save to Collection]
    I --> J[Step 3: Request Permission]
    J --> K[Step 4: Export Content]
    K --> L[Onboarding Complete]
    L --> M[Dashboard with Sample Data]
    M --> N[User Ready to Use Platform]
```

## Flow 7: Content Search and Filter

```mermaid
flowchart TD
    A[Discovery Page] --> B[Enter Search Query]
    B --> C{Search Type?}
    C -->|Hashtag| D[Search by #hashtag]
    C -->|Mention| E[Search by @mention]
    C -->|Keyword| F[Search Captions/Comments]
    D --> G[Execute Search]
    E --> G
    F --> G
    G --> H[Display Initial Results]
    H --> I[Apply Filters]
    I --> J{Filter Type?}
    J -->|Content Type| K[Filter: Photo/Video/Reel]
    J -->|Date Range| L[Filter: Date Range]
    J -->|Engagement| M[Filter: Min Engagement]
    K --> N[Update Results]
    L --> N
    M --> N
    N --> O[Display Filtered Results]
    O --> P{More Results?}
    P -->|Yes| Q[Load More]
    P -->|No| R[End of Results]
    Q --> O
```

## Flow 8: Collection Management

```mermaid
flowchart TD
    A[Collections Page] --> B[View All Collections]
    B --> C{Action?}
    C -->|Create| D[Click New Collection]
    C -->|View| E[Click Collection]
    C -->|Edit| F[Click Edit]
    C -->|Delete| G[Click Delete]
    D --> H[Create Collection Modal]
    H --> I[Enter Name & Description]
    I --> J{Add Content Now?}
    J -->|Yes| K[Browse Content Library]
    J -->|No| L[Create Empty Collection]
    K --> M[Select Content Items]
    M --> L
    L --> N[Collection Created]
    E --> O[View Collection Contents]
    F --> P[Edit Collection Details]
    P --> Q[Save Changes]
    G --> R[Confirm Deletion]
    R --> S[Collection Deleted]
    N --> B
    O --> B
    Q --> B
    S --> B
```

## Flow 9: Content Usage Tracking

```mermaid
flowchart TD
    A[Content Detail View] --> B[View Usage History]
    B --> C{Usage Recorded?}
    C -->|Yes| D[Display Usage Log]
    C -->|No| E[No Usage Recorded]
    D --> F[View Usage Details]
    F --> G{Add Usage?}
    G -->|Yes| H[Click Log Usage]
    G -->|No| I[Close]
    H --> J[Usage Log Modal]
    J --> K[Select Usage Type]
    K --> L[Enter Campaign/Project Name]
    L --> M[Select Date Used]
    M --> N[Add Notes]
    N --> O[Save Usage Record]
    O --> P[Usage Added to History]
    P --> D
    E --> G
```

## Flow 10: Error Handling

```mermaid
flowchart TD
    A[User Action] --> B{Action Type?}
    B -->|API Call| C[Make API Request]
    B -->|Export| D[Process Export]
    B -->|Permission| E[Send Permission Request]
    C --> F{Success?}
    D --> G{Success?}
    E --> H{Success?}
    F -->|No| I[Handle API Error]
    G -->|No| J[Handle Export Error]
    H -->|No| K[Handle Permission Error]
    I --> L{Error Type?}
    L -->|Rate Limit| M[Show Rate Limit Message]
    L -->|Network| N[Show Network Error]
    L -->|Auth| O[Redirect to Login]
    M --> P[Retry After Delay]
    N --> Q[Retry Now]
    O --> R[Re-authenticate]
    J --> S[Show Export Error]
    S --> T[Allow Retry]
    K --> U[Show Permission Error]
    U --> V[Allow Retry]
    P --> C
    Q --> C
    R --> C
    T --> D
    V --> E
```

## Component Interaction Diagram

```mermaid
graph TB
    subgraph "Frontend Components"
        A[Dashboard Component]
        B[Discovery Component]
        C[Library Component]
        D[Detail Modal Component]
        E[Permission Component]
        F[Export Component]
        G[Collection Component]
    end
    
    subgraph "State Management"
        H[React Query Cache]
        I[Zustand Store]
    end
    
    subgraph "API Layer"
        J[Content API]
        K[Permission API]
        L[Export API]
        M[Collection API]
    end
    
    subgraph "External Services"
        N[Instagram Graph API]
        O[Email Service]
        P[S3 Storage]
    end
    
    A --> H
    B --> H
    C --> H
    D --> I
    E --> I
    F --> I
    G --> I
    
    H --> J
    H --> K
    H --> L
    H --> M
    
    J --> N
    K --> O
    L --> P
    M --> P
    
    I --> H
```

## Data Flow Diagram

```mermaid
flowchart LR
    A[User Input] --> B[React Component]
    B --> C{Action Type?}
    C -->|Read| D[React Query]
    C -->|Write| E[Zustand Action]
    D --> F[API Client]
    E --> F
    F --> G[FastAPI Backend]
    G --> H{Operation?}
    H -->|Read| I[PostgreSQL]
    H -->|Write| I
    H -->|External| J[Instagram API]
    H -->|Storage| K[AWS S3]
    I --> L[Response]
    J --> L
    K --> L
    L --> M[Backend Response]
    M --> N[React Query Cache]
    N --> O[Component Update]
    O --> P[UI Rendered]
```

