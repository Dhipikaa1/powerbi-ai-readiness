# 🏗️ Architecture

## Pipeline Flow

```mermaid
flowchart LR
    A[Power BI<br/>.pbip / TMDL] --> B[1. Cleanup]
    B --> C[2. Optimization]
    C --> D[3. BPA Rules]
    D --> E[4. Describe<br/>LLM]
    E --> F[5. Rename<br/>LLM]
    F --> G[6. AI-Readiness<br/>Score]
    G --> H[✅ Copilot-ready<br/>Q&A-ready<br/>Agent-ready]

    style A fill:#fff3e0
    style H fill:#c8e6c9
    style B fill:#e3f2fd
    style C fill:#e3f2fd
    style D fill:#e3f2fd
    style E fill:#f3e5f5
    style F fill:#f3e5f5
    style G fill:#fce4ec
```

## Component Diagram

```mermaid
flowchart TB
    subgraph Inputs
        PBIP[.pbip Project]
        TMDL[TMDL Files]
        REPORT[Report JSON]
        VPA[VertiPaq Analyzer]
    end

    subgraph Tools
        TE[Tabular Editor 2/3]
        LLM[OpenAI / Claude / Azure OpenAI]
        FABRIC[Fabric Notebook + sempy]
    end

    subgraph Pipeline [6-step Pipeline]
        S1[Cleanup]
        S2[Optimization]
        S3[BPA]
        S4[Describe]
        S5[Rename]
        S6[Score]
    end

    subgraph Outputs
        CLEAN[Cleaned TMDL]
        REPORTS[Markdown Reports]
        HTML[HTML Score Report]
        LAKE[(Lakehouse<br/>Score History)]
    end

    PBIP --> S1
    TMDL --> S1
    REPORT --> S1
    VPA --> S2
    TE --> S3
    LLM --> S4
    LLM --> S5
    FABRIC --> S6

    S1 --> CLEAN
    S2 --> CLEAN
    S3 --> REPORTS
    S4 --> CLEAN
    S5 --> CLEAN
    S6 --> HTML
    S6 --> LAKE
```

## Sequence — single dataset run

```mermaid
sequenceDiagram
    actor User
    participant Repo as This Repo
    participant LLM as LLM API
    participant Fabric as Fabric Workspace

    User->>Repo: Run cleanup.py on .pbip
    Repo->>Repo: Parse TMDL + Report JSON
    Repo-->>User: cleanup-report.md (preview)
    User->>Repo: Approve & apply
    Repo->>Repo: Patch TMDL

    User->>Repo: Run describe.py
    Repo->>LLM: Batch measures (20 at a time)
    LLM-->>Repo: Descriptions + synonyms
    Repo->>Repo: Write back to TMDL

    User->>Repo: Run rename.py
    Repo->>LLM: Batch names
    LLM-->>Repo: Suggested renames + impact
    Repo-->>User: rename-mapping.csv (preview)
    User->>Repo: Approve & apply
    Repo->>Repo: Patch TMDL + dependent DAX

    User->>Fabric: Open scoring notebook
    Fabric->>Fabric: Read model via sempy
    Fabric-->>User: 0–100 score + HTML report
```

## Tech Stack

| Layer | Tool |
|-------|------|
| Model parsing | TMDL Python parser (custom) + lxml |
| BPA | Tabular Editor 2 (CLI) or 3 (GUI) |
| LLM | OpenAI GPT-4o-mini / Claude Sonnet / Azure OpenAI |
| Scoring | sempy in Fabric notebook |
| Storage | Fabric Lakehouse (score history) |
| Orchestration | Python scripts or Fabric pipelines |
