# Windows Web App Diagnostic Tool - Workflow Visualization

## Diagnostic Flow Chart

```mermaid
flowchart TD
    A[Start: Web App Analysis] --> B{Windows App with<br/>Basic+ Service Plan?}
    B -->|No| C[❌ Unsupported Platform]
    B -->|Yes| D[Step 1: Initial Health Check]
    
    D --> E{Permission Issues?}
    E -->|Yes| F[🔍 Help Identify Azure Tenant]
    E -->|No| G[Step 2: Availability Analysis]
    F --> G
    
    G --> H[Calculate Request Success Rate<br/>over Time Period]
    H --> I{Availability ≥ 99.99%?}
    I -->|Yes| J[✅ App Healthy - Analysis Complete]
    I -->|No| K[Step 3: Failure Investigation]
    
    K --> L[Identify Top HTTP Error Codes]
    L --> M[Prioritize by Failure Frequency<br/>Focus on 95% of Issues]
    M --> N[Step 4: Root Cause Analysis]
    
    N --> O{Error Type?}
    O -->|HTTP 500| P[Internal Server Error Analysis]
    O -->|HTTP 502| Q[Bad Gateway Analysis]
    O -->|HTTP 503| R[Service Unavailable Analysis]
    
    %% HTTP 500 Analysis Branch
    P --> S[Always Scan Event Logs for .NET Errors]
    S --> T{App Insights Available?}
    T -->|Yes| U[Query Application Insights<br/>for Exception Telemetry]
    T -->|No| V{.NET Application Detected?}
    U --> V
    
    V -->|Yes| W[Collect Profiler Traces<br/>with Failed Request Polling]
    V -->|No| X[Continue with Event Log Analysis]
    W --> Y{Error Sub-Status?}
    X --> Y
    
    Y -->|500.30| Z[ASP.NET Core Startup Failure]
    Y -->|500.0| AA[Application Code Errors]
    Y -->|500.11-15| BB[Application Restart Issues]
    Y -->|500.19-24| CC[Configuration Errors]
    Y -->|500.31-38| DD[ASP.NET Core Module Issues]
    Y -->|500.50-53| EE[URL Rewrite Problems]
    Y -->|500.70-72| FF[Client Certificate Auth]
    Y -->|500.73-79| GG[EasyAuth Authentication]
    Y -->|500.121| HH[Request Timeout Issues]
    Y -->|500.1000-1020| II[IIS Node.js Issues]
    
    %% Special 500.30 handling
    Z --> JJ{Event Logs Have<br/>Startup Exceptions?}
    JJ -->|Yes| KK[Analyze Event Log Details]
    JJ -->|No| LL[ASP.NET Core Startup Simulation]
    LL --> MM[Execute Startup Command<br/>Capture Real Failure Details]
    MM --> NN{Deployment Issues<br/>Suspected?}
    NN -->|Yes| OO[Run from Package Analysis]
    NN -->|No| PP[Provide Startup Error Analysis]
    OO --> QQ[Test Package URL Accessibility<br/>Verify Zip Integrity<br/>Check Configuration]
    
    %% HTTP 502 Analysis Branch
    Q --> RR[Get HTTP 503 Detector Data<br/>for Detailed Analysis]
    RR --> SS[Analyze Autoheal Events]
    SS --> TT[Analyze Worker Process Crashes<br/>Event Logs Disabled]
    TT --> UU[Analyze Low Memory Conditions]
    UU --> VV{Found Specific Issues?}
    VV -->|Yes| WW[Provide Targeted 502 Analysis]
    VV -->|No| XX[General Startup/Connectivity Guidance]
    
    %% HTTP 503 Analysis Branch  
    R --> YY[Check Module-Specific Failures<br/>ASP.NET Core Module]
    YY --> ZZ{Module Issues Found?}
    ZZ -->|Yes| AAA[Analyze Module Failures<br/>Event Logs Enabled]
    ZZ -->|No| BBB[Check Sub-Status Codes]
    
    BBB --> CCC{Sub-Status Code?}
    CCC -->|503.2| DDD[Concurrent Requests Limit]
    CCC -->|503.3| EEE[ASP.NET Queue Full]
    CCC -->|503.4| FFF[FastCGI Queue Full]
    CCC -->|503.12| GGG[WebSocket Disabled]
    CCC -->|503.13| HHH[WebSocket Limit Reached]
    CCC -->|Other/None| III[Comprehensive Resource Analysis]
    
    III --> JJJ[Analyze Autoheal Events]
    JJJ --> KKK[Analyze Worker Process Crashes<br/>Event Logs Disabled]
    KKK --> LLL[Analyze Low Memory Conditions]
    LLL --> MMM{Found Specific Issues?}
    MMM -->|Yes| NNN[Provide Targeted 503 Analysis]
    MMM -->|No| OOO[General Resource Exhaustion Guidance]
    
    %% Final outputs
    KK --> PPP[📊 Generate Analysis Report]
    PP --> PPP
    QQ --> PPP
    AA --> PPP
    BB --> PPP
    CC --> PPP
    DD --> PPP
    EE --> PPP
    FF --> PPP
    GG --> PPP
    HH --> PPP
    II --> PPP
    WW --> PPP
    XX --> PPP
    AAA --> PPP
    DDD --> PPP
    EEE --> PPP
    FFF --> PPP
    GGG --> PPP
    HHH --> PPP
    NNN --> PPP
    OOO --> PPP
    
    PPP --> QQQ[✅ Root Cause Identification<br/>📋 Actionable Recommendations<br/>📈 Priority-Ranked Issues]
    
    %% Styling
    classDef startEnd fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef decision fill:#fff3e0,stroke:#ef6c00,stroke-width:2px
    classDef process fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef analysis fill:#e8f5e8,stroke:#2e7d32,stroke-width:2px
    classDef error fill:#ffebee,stroke:#c62828,stroke-width:2px
    classDef output fill:#e0f2f1,stroke:#00695c,stroke-width:2px
    classDef wip fill:#fafafa,stroke:#757575,stroke-width:2px,stroke-dasharray: 5 5
    
    class A,J,QQQ startEnd
    class B,E,I,T,V,Y,JJ,NN,ZZ,CCC,MMM decision
    class D,G,H,K,L,M,N,S,U,W,X,LL,MM,OO,QQ,RR,YY,BBB,III,JJJ,KKK,LLL process
    class P,Q,R,Z,AA,BB,CC,DD,EE,FF,GG,HH,II,KK,PP,SS,TT,UU,VV,WW,XX,AAA,DDD,EEE,FFF,GGG,HHH,NNN,OOO analysis
    class C error
    class PPP,F output
```

## Data Collection Timeline

```mermaid
gantt
    title Data Collection and Analysis Process
    dateFormat X
    axisFormat %s
    
    section Health Check
    Platform Validation     :0, 5
    Permission Check        :5, 10
    
    section Availability Analysis
    Request Traffic Scan    :10, 30
    Success Rate Calculation :25, 35
    
    section Failure Investigation
    Error Code Identification :35, 45
    Priority Analysis        :40, 50
    
    section Data Collection (Parallel)
    Event Logs Scanning     :50, 80
    App Insights Query      :50, 70
    Profiler Trace Collection :60, 120
    Failed Request Polling  :60, 120
    
    section Special Analysis
    ASP.NET Core Simulation :80, 100
    Package Deployment Check :85, 105
    
    section Report Generation
    Root Cause Analysis     :120, 140
    Recommendations         :135, 150
```

## Error Classification Matrix

```mermaid
graph LR
    A[HTTP Errors] --> B[500 Internal Server]
    A --> C[502 Bad Gateway]
    A --> D[503 Service Unavailable]
    
    B --> E[Application Code<br/>500.0]
    B --> F[Startup Issues<br/>500.11-15]
    B --> G[Configuration<br/>500.19-24]
    B --> H[ASP.NET Core<br/>500.30-39]
    B --> I[URL Rewrite<br/>500.50-53]
    B --> J[Authentication<br/>500.70-79]
    B --> K[Timeouts<br/>500.121]
    B --> L[Node.js<br/>500.1000-1020]
    
    C --> M[Comprehensive Startup Analysis<br/>Uses 503 Detector Data]
    
    D --> N[Concurrent Limits<br/>503.2]
    D --> O[Queue Full<br/>503.3-4]
    D --> P[WebSocket Issues<br/>503.12-13]
    D --> Q[Comprehensive Resource Analysis<br/>Autoheal + Crashes + Memory]
    
    classDef category fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef subcategory fill:#f1f8e9,stroke:#388e3c,stroke-width:1px
    classDef wip fill:#fafafa,stroke:#757575,stroke-width:1px,stroke-dasharray: 3 3
    
    class A,B,C,D category
    class E,F,G,H,I,J,K,N,O,P,Q subcategory
```

## Key Features

### 🎯 **Smart Decision Making**
- Automatic platform validation
- Availability-based analysis depth
- Priority-focused investigation

### 🔍 **Multi-Source Analysis**
- Event Logs scanning (configurable depth)
- Application Insights integration
- Profiler trace collection
- Real-time failure monitoring
- **Autoheal events analysis** for automatic restart detection
- **Worker process crash analysis** with conditional event log scanning
- **Low memory condition monitoring** for resource exhaustion

### 🛠️ **Specialized Tools**
- ASP.NET Core startup simulation
- Run from Package deployment analysis
- Failed request polling
- Cross-tenant resource detection
- **Comprehensive HTTP 502/503 analysis** with SharedAnalyzer integration
- **Prioritized diagnostic flow** preventing duplicate analysis

### 📊 **Comprehensive Coverage**
- 20+ HTTP sub-status codes
- .NET application focus
- Deployment issue detection
- Authentication problem analysis
- **WebSocket-specific diagnostics** (503.12, 503.13)
- **Resource exhaustion analysis** (autoheal, crashes, memory pressure)
- **Module-specific failure detection** (ASP.NET Core Module)

### ⚡ **Current Limitations**
- Performance issues not yet supported
- Focus on availability over latency
- **Note**: HTTP 502/503 analysis now includes comprehensive resource analysis through SharedAnalyzer integration

---

*This diagram shows the complete diagnostic workflow. GitHub will render these Mermaid diagrams automatically when viewing the markdown file.*
