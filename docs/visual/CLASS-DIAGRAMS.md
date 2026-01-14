# UPS Class & Entity Diagrams

## Understanding UPS Structure Through Diagrams

This document shows the relationships between different parts of UPS using class diagrams, entity-relationship diagrams, and component diagrams.

---

## 📊 Table of Contents

1. [Core Schema Structure](#1-core-schema-structure)
2. [Entity Relationship Diagram](#2-entity-relationship-diagram)
3. [Component Architecture](#3-component-architecture)
4. [Data Flow Diagrams](#4-data-flow-diagrams)
5. [State Diagrams](#5-state-diagrams)
6. [Sequence Diagrams](#6-sequence-diagrams)

---

## 1. Core Schema Structure

### 1.1 UPS Spec Class Diagram

```mermaid
classDiagram
    class UPSSpec {
        +String ups_version
        +Metadata metadata
        +ParserDefinition parser
        +ConformanceSuite conformance
        +QualityRequirements quality
        +Extensions extensions
    }

    class Metadata {
        +URI id
        +String name
        +String display_name
        +SemanticVersion version
        +Status status
        +Category category
        +String domain
        +String[] tags
        +String description
        +License license
        +Author[] authors
        +Reference[] references
        +DateTime created_at
        +DateTime updated_at
    }

    class ParserDefinition {
        +InputDefinition input
        +OutputDefinition output
        +ParserMode[] modes
        +ParserFeatures features
        +ErrorHandling error_handling
        +StateMachine state_machine
        +ParserDependency[] dependencies
    }

    class ConformanceSuite {
        +SemanticVersion version
        +TestImport[] imports
        +TestVector[] test_vectors
        +TestGroup[] test_groups
        +PropertyTest[] property_tests
        +FuzzingConfig fuzzing
        +BenchmarkCase[] benchmarks
    }

    class QualityRequirements {
        +ConformanceReq conformance
        +PerformanceReq performance
        +SecurityReq security
        +MaintainabilityReq maintainability
    }

    UPSSpec *-- Metadata
    UPSSpec *-- ParserDefinition
    UPSSpec *-- ConformanceSuite
    UPSSpec *-- QualityRequirements
```

### 1.2 Input/Output Structure

```mermaid
classDiagram
    class InputDefinition {
        +GrammarDefinition format
        +EncodingSpec encoding
        +StreamingSpec streaming
        +FramingSpec framing
        +InputConstraints constraints
        +PreprocessingStep[] preprocessing
    }

    class GrammarDefinition {
        +GrammarType type
        +String variant
        +String grammar
        +String grammar_file
        +String entry_rule
        +BinaryStructure binary_structure
    }

    class OutputDefinition {
        +OutputFormat primary
        +OutputFormat[] alternatives
        +ErrorFormat errors
        +EventStream events
        +Boolean partial_results
    }

    class OutputFormat {
        +OutputType type
        +Schema schema
        +String schema_type
        +NodeType[] node_types
        +String[] preserves
    }

    InputDefinition *-- GrammarDefinition
    InputDefinition *-- EncodingSpec
    InputDefinition *-- StreamingSpec

    OutputDefinition *-- OutputFormat
    OutputDefinition *-- ErrorFormat
```

### 1.3 Grammar Types Hierarchy

```mermaid
classDiagram
    class GrammarType {
        <<enumeration>>
        ABNF
        EBNF
        PEG
        REGEX
        ANTLR4
        TREE_SITTER
        KAITAI
        ASN1
        JSON_SCHEMA
        CUSTOM
        PROSE
    }

    class GrammarDefinition {
        +GrammarType type
        +String grammar
    }

    class ABNFGrammar {
        +String rules
        +String entry_rule
    }

    class PEGGrammar {
        +String rules
        +String entry_rule
        +Boolean packrat
    }

    class BinaryStructure {
        +Endianness endianness
        +Integer alignment
        +MagicBytes magic_bytes
        +BinaryField[] fields
    }

    GrammarDefinition <|-- ABNFGrammar
    GrammarDefinition <|-- PEGGrammar
    GrammarDefinition <|-- BinaryStructure
```

---

## 2. Entity Relationship Diagram

### 2.1 Platform Entities

```mermaid
erDiagram
    SPEC ||--o{ IMPLEMENTATION : "has many"
    SPEC ||--o{ TEST_VECTOR : "contains"
    SPEC ||--|| METADATA : "has"
    SPEC ||--o| CONFORMANCE_SUITE : "may have"

    IMPLEMENTATION ||--o{ QUALITY_SCORE : "receives"
    IMPLEMENTATION ||--o{ CHALLENGE : "participates in"
    IMPLEMENTATION }|--|| AUTHOR : "created by"

    CHALLENGE ||--|{ VOTE : "receives"
    CHALLENGE ||--|| SPEC : "for"

    USER ||--o{ VOTE : "casts"
    USER ||--o{ IMPLEMENTATION : "submits"
    USER ||--o{ SPEC : "creates"

    SPEC {
        uuid id PK
        string name
        semver version
        json definition
        datetime created_at
    }

    IMPLEMENTATION {
        uuid id PK
        uuid spec_id FK
        string language
        semver version
        string source_url
        status status
    }

    QUALITY_SCORE {
        uuid id PK
        uuid impl_id FK
        decimal conformance
        decimal performance
        decimal security
        decimal composite
        integer rank
    }

    CHALLENGE {
        uuid id PK
        uuid spec_id FK
        uuid challenger_id FK
        uuid defender_id FK
        status status
        json results
    }

    USER {
        uuid id PK
        string username
        string email
        decimal credibility
    }

    VOTE {
        uuid id PK
        uuid challenge_id FK
        uuid user_id FK
        uuid choice_id FK
        decimal weight
    }
```

### 2.2 Test Vector Relationships

```mermaid
erDiagram
    CONFORMANCE_SUITE ||--o{ TEST_GROUP : "organizes"
    CONFORMANCE_SUITE ||--o{ TEST_IMPORT : "imports"
    CONFORMANCE_SUITE ||--o| FUZZING_CONFIG : "may have"

    TEST_GROUP ||--o{ TEST_VECTOR : "contains"

    TEST_VECTOR ||--|| TEST_INPUT : "has"
    TEST_VECTOR ||--|| TEST_EXPECTATION : "expects"

    TEST_IMPORT ||--o{ TEST_VECTOR : "provides"

    CONFORMANCE_SUITE {
        semver version
        datetime created_at
    }

    TEST_GROUP {
        string id
        string name
        string description
    }

    TEST_VECTOR {
        string id
        string name
        category category
        priority priority
    }

    TEST_INPUT {
        string inline
        string file
        string url
    }

    TEST_EXPECTATION {
        json success
        json error
        boolean impl_defined
    }
```

---

## 3. Component Architecture

### 3.1 System Components

```mermaid
graph TB
    subgraph "🌐 External"
        USER[👤 Users]
        CI[🔄 CI/CD Systems]
        IDE[💻 IDEs]
    end

    subgraph "🖥️ Frontend"
        WEB[🌐 Web App]
        CLI[⌨️ CLI Tool]
        API_CLIENT[📱 API Client]
    end

    subgraph "⚙️ API Layer"
        API[🔌 REST API]
        GQL[📊 GraphQL API]
        WS[🔗 WebSocket]
    end

    subgraph "🧠 Core Services"
        SPEC_SVC[📋 Spec Service]
        IMPL_SVC[🔧 Implementation Service]
        QUAL_SVC[📊 Quality Service]
        CHALLENGE_SVC[🏆 Challenge Service]
        AUTH_SVC[🔐 Auth Service]
    end

    subgraph "⚡ Processing"
        TEST_RUNNER[✅ Test Runner]
        BENCHMARK[⚡ Benchmark Engine]
        SECURITY[🔒 Security Scanner]
        AI_TESTER[🤖 AI Test Generator]
    end

    subgraph "💾 Data"
        DB[(🗄️ PostgreSQL)]
        CACHE[(⚡ Redis)]
        QUEUE[📨 Message Queue]
        STORAGE[📦 Blob Storage]
    end

    USER --> WEB
    USER --> CLI
    CI --> API
    IDE --> API_CLIENT

    WEB --> API
    CLI --> API
    API_CLIENT --> API

    API --> SPEC_SVC
    API --> IMPL_SVC
    API --> QUAL_SVC
    API --> CHALLENGE_SVC
    API --> AUTH_SVC

    SPEC_SVC --> DB
    IMPL_SVC --> DB
    QUAL_SVC --> CACHE
    CHALLENGE_SVC --> QUEUE

    QUAL_SVC --> TEST_RUNNER
    QUAL_SVC --> BENCHMARK
    QUAL_SVC --> SECURITY
    QUAL_SVC --> AI_TESTER

    TEST_RUNNER --> STORAGE
    BENCHMARK --> STORAGE
```

### 3.2 Quality Engine Components

```mermaid
graph TB
    subgraph "📊 Quality Engine"
        ORCHESTRATOR[🎯 Orchestrator]

        subgraph "Testing Components"
            CONFORM[✅ Conformance<br/>Test Runner]
            PROPERTY[🔄 Property<br/>Test Runner]
            FUZZ[💥 Fuzzer]
        end

        subgraph "Measurement Components"
            BENCH[⚡ Benchmark<br/>Runner]
            MEMORY[💾 Memory<br/>Profiler]
            PERF[📈 Performance<br/>Analyzer]
        end

        subgraph "Security Components"
            VULN[🔍 Vulnerability<br/>Scanner]
            STATIC[📝 Static<br/>Analyzer]
            SAST[🛡️ SAST Tool]
        end

        subgraph "AI Components"
            EDGE[🤖 Edge Case<br/>Generator]
            MUTATE[🧬 Input<br/>Mutator]
        end

        SCORER[📊 Score<br/>Calculator]
    end

    ORCHESTRATOR --> CONFORM
    ORCHESTRATOR --> PROPERTY
    ORCHESTRATOR --> FUZZ

    ORCHESTRATOR --> BENCH
    ORCHESTRATOR --> MEMORY
    ORCHESTRATOR --> PERF

    ORCHESTRATOR --> VULN
    ORCHESTRATOR --> STATIC
    ORCHESTRATOR --> SAST

    ORCHESTRATOR --> EDGE
    ORCHESTRATOR --> MUTATE

    CONFORM --> SCORER
    PROPERTY --> SCORER
    FUZZ --> SCORER
    BENCH --> SCORER
    VULN --> SCORER

    style ORCHESTRATOR fill:#e3f2fd
    style SCORER fill:#c8e6c9
```

---

## 4. Data Flow Diagrams

### 4.1 Spec Creation Flow

```mermaid
flowchart LR
    subgraph "Author"
        A1[📝 Write Spec]
        A2[✅ Validate]
    end

    subgraph "Platform"
        P1[📥 Receive]
        P2[🔍 Schema Check]
        P3[💾 Store]
        P4[📢 Index]
    end

    subgraph "Registry"
        R1[(📚 Spec DB)]
        R2[🔎 Search Index]
    end

    A1 --> A2 --> P1 --> P2
    P2 -->|Valid| P3 --> P4
    P2 -->|Invalid| ERR[❌ Error]
    P3 --> R1
    P4 --> R2

    style ERR fill:#ffcdd2
```

### 4.2 Quality Scoring Flow

```mermaid
flowchart TB
    subgraph "Input"
        IMPL[🔧 Implementation]
        SPEC[📋 Spec]
    end

    subgraph "Testing Phase"
        T1[Run Conformance Tests]
        T2[Run Property Tests]
        T3[Run Fuzzer]
        T4[Run Benchmarks]
        T5[Run Security Scan]
    end

    subgraph "Scoring Phase"
        S1[Calculate Conformance Score]
        S2[Calculate Performance Score]
        S3[Calculate Security Score]
        S4[Apply Weights]
        S5[Calculate Composite]
    end

    subgraph "Output"
        OUT[📊 Quality Score]
        RANK[🏆 Ranking]
    end

    IMPL --> T1 & T4 & T5
    SPEC --> T1 & T2 & T3

    T1 --> S1
    T2 --> S1
    T3 --> S3
    T4 --> S2
    T5 --> S3

    S1 --> S4
    S2 --> S4
    S3 --> S4

    S4 --> S5 --> OUT --> RANK
```

### 4.3 Challenge Flow

```mermaid
flowchart TB
    subgraph "Initiation"
        CH[🆕 Challenger Submits]
        DEF[🏆 Current Champion]
    end

    subgraph "Verification"
        V1{Conformance<br/>100%?}
    end

    subgraph "Competition"
        C1[⚡ Benchmark Both]
        C2[🔒 Security Scan Both]
        C3[🤖 AI Edge Cases]
    end

    subgraph "Decision"
        D1[📊 Compare Scores]
        D2[🗳️ Community Vote]
        D3{Winner?}
    end

    subgraph "Result"
        W1[🏆 New Champion]
        W2[Champion Stays]
    end

    CH --> V1
    V1 -->|No| REJECT[❌ Rejected]
    V1 -->|Yes| C1

    DEF --> C1
    C1 --> C2 --> C3 --> D1 --> D2 --> D3

    D3 -->|Challenger| W1
    D3 -->|Defender| W2

    style REJECT fill:#ffcdd2
    style W1 fill:#c8e6c9
```

---

## 5. State Diagrams

### 5.1 Spec Lifecycle

```mermaid
stateDiagram-v2
    [*] --> Draft: Create

    Draft --> Experimental: Publish
    Draft --> Draft: Edit

    Experimental --> Stable: Mature
    Experimental --> Draft: Revise
    Experimental --> Deprecated: Abandon

    Stable --> Stable: Minor Update
    Stable --> Deprecated: Supersede

    Deprecated --> Retired: End of Life

    Retired --> [*]

    note right of Draft: Under development
    note right of Experimental: Testing phase
    note right of Stable: Production ready
    note right of Deprecated: Being phased out
```

### 5.2 Implementation Status

```mermaid
stateDiagram-v2
    [*] --> Pending: Submit

    Pending --> Testing: Start Review
    Pending --> Rejected: Invalid

    Testing --> Passed: All Tests Pass
    Testing --> Failed: Tests Fail

    Failed --> Testing: Fix & Resubmit
    Failed --> Rejected: Give Up

    Passed --> Ranked: Add to Leaderboard

    Ranked --> Champion: Win Challenge
    Ranked --> Ranked: Lose Challenge

    Champion --> Ranked: Lose Challenge
    Champion --> Champion: Defend

    Ranked --> Deprecated: Outdated
    Champion --> Deprecated: Superseded

    Deprecated --> [*]: Remove
```

### 5.3 Challenge Status

```mermaid
stateDiagram-v2
    [*] --> Initiated: Challenger Submits

    Initiated --> Verification: Accept
    Initiated --> Rejected: Invalid

    Verification --> Conformance: Pass Check
    Verification --> Rejected: Fail Check

    Conformance --> Performance: All Tests Pass
    Conformance --> Failed: Tests Fail

    Performance --> Security: Benchmarks Done

    Security --> AITesting: Scans Done

    AITesting --> Voting: AI Tests Done

    Voting --> Decision: Voting Period Ends

    Decision --> ChallengerWins: Challenger Better
    Decision --> DefenderWins: Defender Better
    Decision --> Tie: No Clear Winner

    ChallengerWins --> [*]: New Champion
    DefenderWins --> [*]: Status Quo
    Tie --> [*]: Requires Revote
```

---

## 6. Sequence Diagrams

### 6.1 Spec Validation Flow

```mermaid
sequenceDiagram
    actor Author
    participant CLI as UPS CLI
    participant Validator
    participant Schema as JSON Schema
    participant Grammar as Grammar Checker

    Author->>CLI: ups validate my-spec.yaml
    CLI->>Validator: validate(spec)

    Validator->>Schema: checkStructure(spec)
    Schema-->>Validator: structureValid

    Validator->>Grammar: validateGrammar(spec.parser.input)
    Grammar-->>Validator: grammarValid

    Validator->>Validator: checkTestVectors()
    Validator->>Validator: checkReferences()

    alt All Valid
        Validator-->>CLI: ✅ Valid
        CLI-->>Author: Spec is valid!
    else Errors Found
        Validator-->>CLI: ❌ Errors
        CLI-->>Author: Fix these issues: [...]
    end
```

### 6.2 Implementation Submission Flow

```mermaid
sequenceDiagram
    actor Dev as Developer
    participant API
    participant Queue as Job Queue
    participant Runner as Test Runner
    participant Bench as Benchmark Engine
    participant Security as Security Scanner
    participant DB
    participant Notify as Notification Service

    Dev->>API: POST /implementations
    API->>DB: Store implementation metadata
    API->>Queue: Enqueue testing job
    API-->>Dev: 202 Accepted (job_id)

    Queue->>Runner: Run conformance tests
    Runner->>Runner: Execute all test vectors
    Runner-->>Queue: Conformance results

    Queue->>Bench: Run benchmarks
    Bench->>Bench: Measure throughput, latency
    Bench-->>Queue: Performance results

    Queue->>Security: Run security scan
    Security->>Security: Fuzz, vulnerability check
    Security-->>Queue: Security results

    Queue->>DB: Store quality score
    Queue->>DB: Update rankings

    Queue->>Notify: Send notification
    Notify-->>Dev: Email: Your submission scored 87.5!
```

### 6.3 Challenge Process

```mermaid
sequenceDiagram
    actor Challenger
    actor Defender
    participant API
    participant Engine as Quality Engine
    participant AI as AI Tester
    participant Community
    participant DB

    Challenger->>API: POST /challenges
    API->>DB: Create challenge record
    API->>Defender: Notify of challenge

    API->>Engine: Start Phase 1: Conformance
    Engine->>Engine: Run all tests
    Engine-->>API: Phase 1 results

    alt Challenger fails tests
        API-->>Challenger: Challenge rejected
    else Challenger passes
        API->>Engine: Start Phase 2: Benchmark
        Engine->>Engine: Compare performance
        Engine-->>API: Phase 2 results

        API->>Engine: Start Phase 3: Security
        Engine->>Engine: Compare security
        Engine-->>API: Phase 3 results

        API->>AI: Start Phase 4: AI Testing
        AI->>AI: Generate edge cases
        AI->>Engine: Run edge cases on both
        AI-->>API: Phase 4 results

        API->>Community: Start Phase 5: Voting
        loop 7 days
            Community->>API: Cast votes
        end
        API->>API: Tally votes

        API->>DB: Record final result
        API-->>Challenger: Result notification
        API-->>Defender: Result notification
    end
```

---

## 🎨 Color Legend

Throughout these diagrams, we use consistent colors:

| Color | Meaning |
|-------|---------|
| 🔵 Blue (`#e3f2fd`) | Input / Request |
| 🟢 Green (`#c8e6c9`) | Success / Valid |
| 🟡 Yellow (`#fff9c4`) | Processing / In Progress |
| 🔴 Red (`#ffcdd2`) | Error / Failure |
| 🟣 Purple (`#e1bee7`) | Output / Result |
| ⚪ Gray (`#f5f5f5`) | Neutral / Optional |

---

*These diagrams are part of the Universal Parser Specification documentation.*
