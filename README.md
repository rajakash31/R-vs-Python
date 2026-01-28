flowchart TB
  %% =========================
  %% GhoomAI System Architecture (Hierarchical)
  %% =========================

  A["Level 1: GhoomAI — AI Travel Planning System"]

  %% Level 2: Major Subsystems
  A --> FE["Level 2: Frontend Application"]
  A --> BE["Level 2: Backend API (Flask)"]
  A --> AI["Level 2: AI Processing System"]
  A --> DB["Level 2: Database System"]
  A --> ADM["Level 2: Admin & Monitoring System"]
  A --> EXT["Level 2: External Services (Optional)"]

  %% Level 3: Components
  subgraph FE_L3["Level 3: Frontend Components"]
    FE1["User Web UI\n(Trip Input, Itinerary View, Profile, Export)"]
    FE2["Admin Dashboard UI\n(Analytics, Trends, System Health)"]
  end
  FE --> FE_L3

  subgraph BE_L3["Level 3: Backend (Flask) Components"]
    BE1["Auth & RBAC\n(Login/Register, User/Admin Roles)"]
    BE2["Trip Planning Controller\n(Orchestrator)"]
    BE3["User Profile Management\n(Preferences, History)"]
    BE4["PDF Export Service"]
    BE5["Admin Analytics API"]
    BE6["Logging Hooks\n(Latency, Errors)"]
  end
  BE --> BE_L3

  subgraph AI_L3["Level 3: AI Processing Components"]
    AI1["Prompt Builder / NLP Processor"]
    AI2["LLM Integration Layer"]
    AI3["Constraint & Budget Optimizer\n(Time, Cost, Group Size, Interests)"]
    GPT["OpenAI GPT-4"]
    DS["DeepSeek LLM"]
  end
  AI --> AI_L3
  AI2 --> GPT
  AI2 --> DS

  subgraph DB_L3["Level 3: Data Stores (SQLAlchemy)"]
    DB1["Users"]
    DB2["Profiles / Preferences"]
    DB3["Trips"]
    DB4["Itineraries"]
    DB5["Admin Metrics"]
  end
  DB --> DB_L3

  subgraph ADM_L3["Level 3: Admin & Monitoring Components"]
    AD1["Usage Analytics"]
    AD2["System Logs"]
    AD3["Performance Monitoring"]
  end
  ADM --> ADM_L3

  subgraph EXT_L3["Level 3: Optional External APIs"]
    W["Weather API"]
    M["Maps / Traffic API"]
    P["Places / Attractions API"]
  end
  EXT --> EXT_L3

  %% Key Connections
  FE1 -->|HTTPS / REST| BE2
  FE2 -->|HTTPS / REST| BE5

  BE1 --> DB
  BE3 --> DB
  BE2 --> DB
  BE4 --> DB
  BE5 --> DB

  BE2 --> AI1
  AI1 --> AI2
  AI2 --> AI3
  AI3 --> BE2

  BE6 --> AD2
  BE6 --> AD3
  BE5 --> AD1

  %% Optional external calls (low priority)
  BE2 -. optional .-> W
  BE2 -. optional .-> M
  BE2 -. optional .-> P

  %% Bottom AI Flow (Input -> Model -> Output)
  subgraph FLOW["Bottom Layer: AI Processing Flow (Input → Model → Output)"]
    IN["Input:\nDestination, Dates, Budget,\nGroup Size, Interests"]
    MODEL["AI Model(s):\nGPT-4 / DeepSeek\n(via Prompt Builder)"]
    OUT["Output:\nOptimized Day-wise Itinerary\n+ Exportable Plan"]
    IN --> MODEL --> OUT
  end

  FE1 --> IN
  OUT --> FE1
