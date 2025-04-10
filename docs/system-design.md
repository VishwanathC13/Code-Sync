# Video Calling Interview Platform - System Design

## System Overview
The Video Calling Interview Platform is a web-based application that enables video interviews with features like screen sharing, recording, and real-time collaboration. The system is built using Next.js, TypeScript, Stream for video calls, Convex for backend, and Clerk for authentication.

## 1. Class Diagram
```mermaid
classDiagram
    class User {
        +String id
        +String name
        +String email
        +String role
        +joinInterview()
        +leaveInterview()
    }

    class Interview {
        +String id
        +String title
        +DateTime scheduledTime
        +User interviewer
        +User interviewee
        +InterviewStatus status
        +startInterview()
        +endInterview()
        +recordInterview()
    }

    class VideoCall {
        +String id
        +Interview interview
        +Stream stream
        +startCall()
        +endCall()
        +shareScreen()
        +toggleRecording()
    }

    class Recording {
        +String id
        +Interview interview
        +String url
        +DateTime createdAt
        +downloadRecording()
    }

    User "1" -- "0..*" Interview : participates in
    Interview "1" -- "1" VideoCall : has
    Interview "1" -- "0..*" Recording : has
```

## 2. Use Case Diagram
```mermaid
graph TD
    subgraph Actors
        A1[Interviewer]
        A2[Interviewee]
        A3[Admin]
    end

    subgraph Interviewer Actions
        B1[Schedule Interview]
        B2[Start Video Call]
        B3[Share Screen]
        B4[Record Interview]
        B5[End Interview]
    end

    subgraph Interviewee Actions
        C1[Join Interview]
        C2[Participate in Call]
        C3[View Shared Screen]
    end

    subgraph Admin Actions
        D1[Manage Users]
        D2[View Recordings]
        D3[Monitor System]
    end

    A1 --> B1
    A1 --> B2
    A1 --> B3
    A1 --> B4
    A1 --> B5

    A2 --> C1
    A2 --> C2
    A2 --> C3

    A3 --> D1
    A3 --> D2
    A3 --> D3
```

## 3. Activity Diagram
```mermaid
flowchart TD
    A[Start] --> B[User Authentication]
    B --> C{Is Authenticated?}
    C -->|No| D[Redirect to Login]
    C -->|Yes| E[Access Dashboard]
    E --> F[Schedule Interview]
    F --> G[Send Invitation]
    G --> H[Wait for Scheduled Time]
    H --> I[Start Video Call]
    I --> J[Enable Screen Sharing]
    J --> K[Start Recording]
    K --> L[Conduct Interview]
    L --> M[End Recording]
    M --> N[End Call]
    N --> O[Save Recording]
    O --> P[End]
```

## 4. Entity Relationship Diagram
```mermaid
erDiagram
    USER ||--o{ INTERVIEW : participates
    INTERVIEW ||--|| VIDEO_CALL : has
    INTERVIEW ||--o{ RECORDING : has

    USER {
        string id PK
        string name
        string email
        string role
        datetime created_at
    }

    INTERVIEW {
        string id PK
        string title
        string interviewer_id FK
        string interviewee_id FK
        datetime scheduled_time
        string status
        datetime created_at
    }

    VIDEO_CALL {
        string id PK
        string interview_id FK
        string stream_id
        boolean is_recording
        datetime started_at
        datetime ended_at
    }

    RECORDING {
        string id PK
        string interview_id FK
        string url
        datetime created_at
        integer duration
    }
```

## System Components

### Frontend (Next.js)
- User Interface Components
- Video Call Interface
- Authentication Pages
- Dashboard
- Interview Management

### Backend (Convex)
- User Management
- Interview Scheduling
- Video Call Management
- Recording Storage
- Real-time Updates

### External Services
- Stream (Video Calls)
- Clerk (Authentication)
- Storage (Recordings)

## Data Flow
1. User authentication through Clerk
2. Interview scheduling and management
3. Video call initiation through Stream
4. Real-time communication
5. Recording storage and management
6. Post-interview analytics and review

## Security Considerations
- End-to-end encryption for video calls
- Role-based access control
- Secure storage of recordings
- Authentication and authorization
- Data privacy compliance
