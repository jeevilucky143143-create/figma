%%{init: {'theme': 'base', 'themeVariables': {'background': '#ffffff'}}}%%
flowchart TB
    classDef confirmed fill:#d4edda,stroke:#28a745,stroke-width:2px,color:#000
    classDef inferred fill:#d1ecf1,stroke:#17a2b8,stroke-width:2px,color:#000
    classDef hypothetical fill:#fff3cd,stroke:#ffc107,stroke-width:2px,color:#000
    classDef unexamined fill:#e2e3e5,stroke:#6c757d,stroke-width:2px,stroke-dasharray: 5 5,color:#000

    subgraph L1 [1. USER LAYER]
        direction LR
        Designer["Designer / User\n(E-001, E-005)"]:::confirmed
        Browser["Web Browser\n(E-001)"]:::confirmed
    end

    subgraph L2 [2. FRONTEND LAYER]
        direction TB
        Home["Figma Home Page\n(E-001)"]:::confirmed
        Editor["Design Editor\n(E-001)"]:::confirmed
        
        subgraph Workspace [Editor Interface]
            direction LR
            Canvas["Design Canvas\n(E-002)"]:::confirmed
            Layers["Layers Panel\n(E-002)"]:::confirmed
            Comps["Components\n(E-003)"]:::confirmed
            ProtoUI["Prototype Workspace\n(E-004)"]:::confirmed
            CollabUI["Sharing & Comments Interface\n(E-005)"]:::confirmed
        end
    end

    subgraph AuthLayer [AUTHENTICATION & SECURITY]
        direction LR
        UserAuth["User Authentication\n(Not Examined)"]:::unexamined
        FilePerms["File Access Permissions\n(E-005)"]:::confirmed
    end

    subgraph L3 [3. API / COMMUNICATION LAYER]
        direction LR
        AppAPI["Application API / Communication\n(Hypothetical)"]:::hypothetical
        FileOp["File Operations\n(Hypothetical)"]:::hypothetical
        DesignOp["Design Operations\n(Hypothetical)"]:::hypothetical
        CollabOp["Collaboration Requests\n(Hypothetical)"]:::hypothetical
    end

    subgraph L4 [4. BACKEND SERVICES]
        direction LR
        FileMgmt["File Management\n(Inferred)"]:::inferred
        DesignMgmt["Design & Layer Management\n(Inferred)"]:::inferred
        CompMgmt["Component Management\n(Inferred)"]:::inferred
        ProtoMgmt["Prototype Management\n(Inferred)"]:::inferred
        CollabMgmt["Sharing & Comment Management\n(Inferred)"]:::inferred
    end

    subgraph L5 [5. DATA AND STORAGE LAYER]
        direction LR
        FileData[("Design File Data\n(Hypothetical)")]:::hypothetical
        LayerData[("Layers & Component Data\n(Hypothetical)")]:::hypothetical
        ProtoData[("Prototype Connection Data\n(Hypothetical)")]:::hypothetical
        CollabData[("Comments & Sharing Data\n(Hypothetical)")]:::hypothetical
    end

    %% User interactions (Solid)
    Designer -->|Uses| Browser
    Browser -->|Opens| Home
    Home -->|Loads| Editor
    Editor --> Workspace

    %% Frontend to API (Dashed - Internal details unknown)
    Home -.-> AppAPI
    Canvas -.-> DesignOp
    Layers -.-> DesignOp
    Comps -.-> DesignOp
    ProtoUI -.-> DesignOp
    CollabUI -.-> CollabOp
    Workspace -.-> FileOp

    %% API to Security (Dashed)
    AppAPI -.-> UserAuth
    FileOp -.-> FilePerms
    CollabOp -.-> FilePerms

    %% API to Backend (Dashed)
    FileOp -.-> FileMgmt
    DesignOp -.-> DesignMgmt
    DesignOp -.-> CompMgmt
    DesignOp -.-> ProtoMgmt
    CollabOp -.-> CollabMgmt

    %% Backend to Data (Dashed)
    FileMgmt -.-> FileData
    DesignMgmt -.-> LayerData
    CompMgmt -.-> LayerData
    ProtoMgmt -.-> ProtoData
    CollabMgmt -.-> CollabData
