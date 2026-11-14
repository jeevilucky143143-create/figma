flowchart TB
    %% Styling based on Evidence Classification
    classDef confirmed fill:#d4edda,stroke:#28a745,stroke-width:2px,color:#000
    classDef inferred fill:#d1ecf1,stroke:#17a2b8,stroke-width:2px,color:#000
    classDef hypothesis fill:#fff3cd,stroke:#ffc107,stroke-width:2px,color:#000
    classDef unexamined fill:#e2e3e5,stroke:#6c757d,stroke-width:2px,stroke-dasharray: 5 5,color:#000

    subgraph Layer1 ["1. USER & CLIENT LAYER"]
        direction LR
        Users["Designers & Collaborators\n(E-001, E-005)"]:::confirmed
        BrowserApp["Browser-Based Figma Editor\n(E-001)"]:::confirmed
    end

    subgraph Layer2 ["2. FRONTEND / PRESENTATION LAYER"]
        direction TB
        HomeUI["Home Page / File Browser\n(E-001)"]:::confirmed
        CanvasUI["Design Canvas / Rendering\n(E-001, E-002)"]:::confirmed
        LayersUI["Layers Panel\n(E-002)"]:::confirmed
        PropertiesUI["Properties & Components\n(E-003)"]:::confirmed
        PrototypeUI["Present Mode / Interactions\n(E-004)"]:::confirmed
        CommentUI["Collaboration Interface\n(E-005)"]:::confirmed
    end

    subgraph Layer3 ["3. API & COMMUNICATION LAYER"]
        direction LR
        REST_Gateway["API Gateway / Request Router\n(I-001)"]:::inferred
        RealTime_WS["Real-Time Sync Channel\n(I-002)"]:::inferred
    end

    subgraph Layer4 ["4. BACKEND APPLICATION SERVICES"]
        direction TB
        AuthSvc["Auth & Access Control\n(E-001, E-005)"]:::confirmed
        FileSvc["Design File Management\n(E-001)"]:::confirmed
        NodeSvc["Document & Node Management\n(E-002, E-003)"]:::confirmed
        ProtoSvc["Prototype Mapping Service\n(E-004)"]:::confirmed
        CommentSvc["Comment Management\n(E-005)"]:::confirmed
        BgTasks["Thumbnail/Export Processing\n(H-001)"]:::hypothesis
    end

    subgraph Layer5 ["5. REAL-TIME COLLABORATION"]
        Multiplayer["Multiplayer Sync Engine\n(I-003)"]:::inferred
    end

    subgraph Layer6 ["6. DATA & STORAGE LAYER"]
        direction LR
        UserDB[("User & Permissions\n(I-004)")]:::inferred
        DocDB[("Design Document Nodes\n(I-005)")]:::inferred
        CommentDB[("Comments & Feedback\n(I-006)")]:::inferred
        AssetStore[("Blob / Asset Storage\n(H-002)")]:::hypothesis
    end
    
    subgraph Layer7 ["7. EXTERNAL INTEGRATIONS & MONITORING"]
        Plugins["Plugin / External Integrations\n(U-001)"]:::unexamined
        Monitoring["Internal Telemetry\n(U-002)"]:::unexamined
    end

    %% Data Flow & Connections
    Users <-->|Interacts via Browser| BrowserApp
    BrowserApp --> HomeUI & CanvasUI & LayersUI & PropertiesUI & PrototypeUI & CommentUI

    %% Standard HTTP Requests
    HomeUI & PropertiesUI & PrototypeUI -->|REST / HTTP| REST_Gateway
    REST_Gateway --> AuthSvc & FileSvc & ProtoSvc
    
    %% Real-time WebSockets
    CanvasUI & LayersUI & CommentUI <-->|Live Event Stream| RealTime_WS
    RealTime_WS <--> Multiplayer

    %% Backend inter-service
    Multiplayer <--> NodeSvc & CommentSvc
    NodeSvc <--> ProtoSvc
    FileSvc -.-> BgTasks

    %% Storage connections
    AuthSvc --> UserDB
    FileSvc --> DocDB
    NodeSvc --> DocDB
    CommentSvc --> CommentDB
    CanvasUI -.->|Image/Asset Fetch| AssetStore
    BgTasks -.-> AssetStore