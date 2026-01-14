# UPS Runtime Architecture

## Universal Parser Specification Runtime System

**Version:** 1.0.0
**Status:** Draft
**Author:** UPP Architecture Team
**Last Updated:** January 2025

---

## Executive Summary

This document defines the comprehensive architecture for the **UPS Runtime System** - a platform that enables validation, benchmarking, and comparison of parser implementations across any programming language using the Universal Parser Specification (UPS) as the universal contract.

### Core Vision

```
"Write once (UPS Spec), validate everywhere (any language, any library)"
```

The UPS Runtime bridges the gap between:
- **Specification** (what a parser SHOULD do) - defined in UPS YAML/JSON
- **Implementation** (what a parser ACTUALLY does) - existing libraries in npm, NuGet, PyPI, etc.

---

## Table of Contents

1. [Problem Statement](#1-problem-statement)
2. [Solution Overview](#2-solution-overview)
3. [System Architecture](#3-system-architecture)
4. [Core Components](#4-core-components)
5. [Language Adapter Specification](#5-language-adapter-specification)
6. [Package Manager Integration](#6-package-manager-integration)
7. [Execution Engine](#7-execution-engine)
8. [Test & Benchmark Runner](#8-test--benchmark-runner)
9. [Reporting & Comparison](#9-reporting--comparison)
10. [Data Flow](#10-data-flow)
11. [CLI Interface](#11-cli-interface)
12. [Extension Points](#12-extension-points)
13. [Security Considerations](#13-security-considerations)
14. [Implementation Phases](#14-implementation-phases)

---

## 1. Problem Statement

### Current Challenges

1. **No Universal Testing**: Each parser library has its own test suite (if any)
2. **No Standard Comparison**: Impossible to objectively compare CsvHelper vs pandas vs papaparse
3. **Fragmented Quality**: No consistent quality metrics across ecosystems
4. **Manual Evaluation**: Developers spend hours researching which library to use

### What We Need

| Requirement | Solution |
|-------------|----------|
| Universal test definition | UPS Conformance Suite |
| Cross-language execution | Language Adapters |
| Real library integration | Package Manager Connectors |
| Objective comparison | Standardized Metrics |

---

## 2. Solution Overview

### The Universal Bridge Pattern

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        UPS SPECIFICATION LAYER                          │
│                                                                         │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐         │
│  │ csv-parser.ups  │  │ json-parser.ups │  │ xml-parser.ups  │   ...   │
│  │     .yaml       │  │     .yaml       │  │     .yaml       │         │
│  └────────┬────────┘  └────────┬────────┘  └────────┬────────┘         │
│           │                    │                    │                   │
└───────────┼────────────────────┼────────────────────┼───────────────────┘
            │                    │                    │
            ▼                    ▼                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         UPS RUNTIME ENGINE                              │
│                                                                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌────────────┐  │
│  │ Spec Loader  │  │ Test Runner  │  │  Benchmark   │  │  Reporter  │  │
│  │              │  │              │  │    Engine    │  │            │  │
│  └──────────────┘  └──────────────┘  └──────────────┘  └────────────┘  │
│                                                                         │
└─────────────────────────────────┬───────────────────────────────────────┘
                                  │
                    ┌─────────────┼─────────────┐
                    │             │             │
                    ▼             ▼             ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      LANGUAGE ADAPTER LAYER                             │
│                                                                         │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐               │
│  │    Python     │  │    .NET       │  │   Node.js     │     ...       │
│  │    Adapter    │  │    Adapter    │  │    Adapter    │               │
│  └───────┬───────┘  └───────┬───────┘  └───────┬───────┘               │
│          │                  │                  │                        │
└──────────┼──────────────────┼──────────────────┼────────────────────────┘
           │                  │                  │
           ▼                  ▼                  ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                    PACKAGE MANAGER INTEGRATION                          │
│                                                                         │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐               │
│  │     PyPI      │  │    NuGet      │  │     npm       │     ...       │
│  │  ───────────  │  │  ───────────  │  │  ───────────  │               │
│  │  - pandas     │  │  - CsvHelper  │  │  - papaparse  │               │
│  │  - csv        │  │  - Sylvan     │  │  - csv-parse  │               │
│  └───────────────┘  └───────────────┘  └───────────────┘               │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Key Principles

1. **Specification-Driven**: UPS spec is the single source of truth
2. **Adapter Abstraction**: Each language implements a standard adapter interface
3. **Real Libraries**: Test actual production libraries, not toy implementations
4. **Reproducible**: Same spec + same library = same results
5. **Extensible**: Add new languages/libraries without changing core

---

## 3. System Architecture

### High-Level Components

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           UPS RUNTIME SYSTEM                            │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                        CORE ENGINE                               │   │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌───────────┐  │   │
│  │  │    Spec     │ │   Schema    │ │    Test     │ │ Benchmark │  │   │
│  │  │   Loader    │ │  Validator  │ │  Executor   │ │  Runner   │  │   │
│  │  └─────────────┘ └─────────────┘ └─────────────┘ └───────────┘  │   │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌───────────┐  │   │
│  │  │   Result    │ │  Comparison │ │   Report    │ │   Cache   │  │   │
│  │  │  Collector  │ │    Engine   │ │  Generator  │ │  Manager  │  │   │
│  │  └─────────────┘ └─────────────┘ └─────────────┘ └───────────┘  │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                     ADAPTER MANAGER                              │   │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌───────────┐  │   │
│  │  │  Adapter    │ │  Process    │ │    IPC      │ │  Health   │  │   │
│  │  │  Registry   │ │  Manager    │ │   Bridge    │ │  Monitor  │  │   │
│  │  └─────────────┘ └─────────────┘ └─────────────┘ └───────────┘  │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                     CLI INTERFACE                                │   │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌───────────┐  │   │
│  │  │    Test     │ │  Benchmark  │ │   Compare   │ │   List    │  │   │
│  │  │   Command   │ │   Command   │ │   Command   │ │  Command  │  │   │
│  │  └─────────────┘ └─────────────┘ └─────────────┘ └───────────┘  │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Component Responsibilities

| Component | Responsibility |
|-----------|----------------|
| **Spec Loader** | Parse UPS YAML/JSON, resolve references |
| **Schema Validator** | Validate spec against UPS JSON Schema |
| **Test Executor** | Run conformance test_vectors |
| **Benchmark Runner** | Execute performance benchmarks |
| **Result Collector** | Aggregate results from adapters |
| **Comparison Engine** | Compare results across libraries |
| **Report Generator** | Generate human/machine-readable reports |
| **Adapter Registry** | Discover and manage language adapters |
| **Process Manager** | Spawn/manage adapter processes |
| **IPC Bridge** | Communication between engine and adapters |

---

## 4. Core Components

### 4.1 Spec Loader

The Spec Loader reads and normalizes UPS specifications.

```yaml
# Spec Loader Contract
input:
  - UPS YAML/JSON file path
  - Or UPS spec ID (for registry lookup)

output:
  normalized_spec:
    metadata: { ... }
    parser: { ... }
    conformance: { ... }
    quality: { ... }

capabilities:
  - Resolve $ref references
  - Merge imported grammars
  - Validate against UPS schema
  - Cache parsed specs
```

### 4.2 Test Executor

Executes conformance tests defined in UPS spec.

```yaml
# Test Executor Contract
input:
  spec: NormalizedUPSSpec
  adapter: LanguageAdapter
  library: LibraryBinding
  options:
    filter_categories: [valid, invalid, edge-case]
    filter_tags: [...]
    parallel: true
    timeout_ms: 5000

output:
  results:
    - test_id: "valid-001"
      status: pass | fail | skip | error
      duration_ms: 12
      actual_output: { ... }
      expected_output: { ... }
      diff: { ... }  # if failed
      error: { ... } # if error

  summary:
    total: 100
    passed: 95
    failed: 3
    skipped: 2
    errors: 0
    duration_ms: 1234
```

### 4.3 Benchmark Runner

Executes performance benchmarks with statistical rigor.

```yaml
# Benchmark Runner Contract
input:
  spec: NormalizedUPSSpec
  adapter: LanguageAdapter
  library: LibraryBinding
  options:
    warmup_iterations: 100
    measurement_iterations: 1000
    timeout_seconds: 60

output:
  results:
    - benchmark_id: "large-file"
      metrics:
        throughput_mbps:
          mean: 450.5
          stddev: 12.3
          min: 420.1
          max: 478.9
          p50: 451.2
          p99: 475.6
        latency_us:
          mean: 2200
          p50: 2100
          p99: 3500
        memory_mb:
          peak: 256
          average: 128
      passes_minimum: true
      passes_target: false

  environment:
    cpu: "Intel i7-12700"
    memory_gb: 32
    os: "Ubuntu 22.04"
    runtime_version: "Python 3.11"
```

### 4.4 Comparison Engine

Compares results across multiple library implementations.

```yaml
# Comparison Engine Contract
input:
  results:
    - library: "pandas"
      adapter: "python"
      test_results: { ... }
      benchmark_results: { ... }
    - library: "CsvHelper"
      adapter: "dotnet"
      test_results: { ... }
      benchmark_results: { ... }

output:
  comparison:
    conformance_ranking:
      1: { library: "CsvHelper", score: 98.5, passed: 98, failed: 1 }
      2: { library: "pandas", score: 95.0, passed: 95, failed: 5 }

    performance_ranking:
      throughput:
        1: { library: "Sylvan.Data", mbps: 890 }
        2: { library: "pandas", mbps: 450 }
      memory:
        1: { library: "csv-parse", mb: 45 }
        2: { library: "pandas", mb: 256 }

    overall_ranking:
      1: { library: "Sylvan.Data", score: 94.2 }
      2: { library: "CsvHelper", score: 91.5 }
      3: { library: "pandas", score: 87.3 }

    recommendation:
      best_overall: "Sylvan.Data"
      best_conformance: "CsvHelper"
      best_performance: "Sylvan.Data"
      best_memory: "csv-parse"
```

---

## 5. Language Adapter Specification

### 5.1 Adapter Interface Contract

Every language adapter MUST implement this interface:

```yaml
# Universal Adapter Interface (UAI) v1.0
interface: UniversalParserAdapter
version: "1.0"

methods:
  # Initialization
  initialize:
    input:
      config:
        library_name: string
        library_version: string
        options: object
    output:
      success: boolean
      capabilities: string[]
      error: string?

  # Parse operation
  parse:
    input:
      data: bytes | string
      encoding: string
      options: object
    output:
      success: boolean
      result: object  # AST or parsed data
      errors: Error[]
      duration_ns: integer
      memory_bytes: integer

  # Validation (optional)
  validate:
    input:
      data: bytes | string
      schema: object?
    output:
      valid: boolean
      errors: ValidationError[]

  # Serialization (optional)
  serialize:
    input:
      data: object
      options: object
    output:
      result: bytes | string
      success: boolean

  # Metadata
  get_info:
    output:
      adapter_name: string
      adapter_version: string
      language: string
      language_version: string
      supported_libraries: Library[]

  # Health check
  ping:
    output:
      alive: boolean
      uptime_ms: integer
```

### 5.2 IPC Protocol

Communication between UPS Runtime and Adapters uses JSON-RPC over stdin/stdout:

```json
// Request (Runtime → Adapter)
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "parse",
  "params": {
    "data": "a,b,c\n1,2,3",
    "encoding": "utf-8",
    "options": {
      "has_header": true
    }
  }
}

// Response (Adapter → Runtime)
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "success": true,
    "result": {
      "headers": ["a", "b", "c"],
      "records": [["1", "2", "3"]],
      "rowCount": 1
    },
    "duration_ns": 125000,
    "memory_bytes": 1024
  }
}
```

### 5.3 Adapter Implementation Examples

#### Python Adapter Structure

```
adapters/
  python/
    ├── ups_adapter/
    │   ├── __init__.py
    │   ├── main.py           # Entry point (JSON-RPC server)
    │   ├── interface.py      # UAI implementation
    │   └── bindings/
    │       ├── __init__.py
    │       ├── csv_stdlib.py # Python csv module binding
    │       ├── pandas_csv.py # pandas binding
    │       └── polars_csv.py # polars binding
    ├── pyproject.toml
    └── README.md
```

#### .NET Adapter Structure

```
adapters/
  dotnet/
    ├── UpsAdapter/
    │   ├── Program.cs        # Entry point
    │   ├── AdapterInterface.cs
    │   └── Bindings/
    │       ├── CsvHelperBinding.cs
    │       ├── SylvanBinding.cs
    │       └── SystemTextCsvBinding.cs
    ├── UpsAdapter.csproj
    └── README.md
```

### 5.4 Library Binding Specification

Each library binding within an adapter MUST implement:

```yaml
# Library Binding Contract
interface: LibraryBinding

properties:
  library_id: string        # Unique ID: "python:pandas", "dotnet:csvhelper"
  library_name: string      # Display name: "pandas"
  library_version: string   # "2.1.0"
  package_source: string    # "pypi", "nuget", "npm"
  package_name: string      # Package manager name
  supported_formats: string[] # ["csv", "json", "xml"]
  capabilities: string[]    # ["parse", "serialize", "validate", "stream"]

methods:
  parse:
    # Format-specific parsing
    # Returns normalized output matching UPS output schema

  serialize:
    # Convert AST back to format

  get_options:
    # Return available library-specific options
    # Mapped to UPS mode options
```

---

## 6. Package Manager Integration

### 6.1 Package Manager Abstraction

```yaml
# Package Manager Interface
interface: PackageManager

methods:
  search:
    input:
      query: string
      format: string?  # "csv", "json", etc.
    output:
      packages: Package[]

  get_package_info:
    input:
      name: string
    output:
      name: string
      version: string
      description: string
      downloads: integer
      repository: string
      license: string

  install:
    input:
      name: string
      version: string?
    output:
      success: boolean
      installed_path: string

  list_installed:
    output:
      packages: InstalledPackage[]
```

### 6.2 Supported Package Managers

| Language | Package Manager | Config File | Install Command |
|----------|-----------------|-------------|-----------------|
| Python | PyPI | pyproject.toml | `pip install` |
| .NET | NuGet | *.csproj | `dotnet add package` |
| Node.js | npm | package.json | `npm install` |
| Rust | crates.io | Cargo.toml | `cargo add` |
| Go | go modules | go.mod | `go get` |
| Java | Maven Central | pom.xml | `mvn install` |

### 6.3 Library Registry

The system maintains a registry of known parser libraries:

```yaml
# Library Registry Schema
libraries:
  - id: "python:pandas"
    name: "pandas"
    language: python
    package_manager: pypi
    package_name: "pandas"
    formats:
      - csv
      - json
      - xml
      - excel
    binding_class: "ups_adapter.bindings.pandas_csv.PandasCsvBinding"
    min_version: "1.0.0"
    recommended_version: "2.1.0"

  - id: "dotnet:csvhelper"
    name: "CsvHelper"
    language: dotnet
    package_manager: nuget
    package_name: "CsvHelper"
    formats:
      - csv
    binding_class: "UpsAdapter.Bindings.CsvHelperBinding"
    min_version: "30.0.0"

  - id: "node:papaparse"
    name: "Papa Parse"
    language: nodejs
    package_manager: npm
    package_name: "papaparse"
    formats:
      - csv
    binding_class: "bindings/papaparse.js"
    min_version: "5.0.0"
```

### 6.4 Auto-Discovery

The system can auto-discover libraries:

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  Package        │     │   Library       │     │   Binding       │
│  Manager API    │────▶│   Classifier    │────▶│   Generator     │
│                 │     │                 │     │   (optional)    │
└─────────────────┘     └─────────────────┘     └─────────────────┘
        │                       │                       │
        ▼                       ▼                       ▼
   Search for             Detect parser           Generate stub
   "csv parser"           capabilities            binding code
```

---

## 7. Execution Engine

### 7.1 Execution Modes

```yaml
execution_modes:
  # Run in same process (if adapter supports)
  in_process:
    pros: [fast, low_overhead]
    cons: [language_specific, crash_affects_host]
    use_when: same_language_as_runtime

  # Run adapter as subprocess
  subprocess:
    pros: [isolated, crash_safe, any_language]
    cons: [ipc_overhead, startup_time]
    use_when: different_language

  # Run in container
  container:
    pros: [fully_isolated, reproducible, any_dependencies]
    cons: [highest_overhead, requires_docker]
    use_when: complex_dependencies, ci_environment

  # Run via remote service
  remote:
    pros: [scalable, shared_infrastructure]
    cons: [network_latency, requires_service]
    use_when: cloud_deployment, distributed_testing
```

### 7.2 Process Management

```
┌─────────────────────────────────────────────────────────────────┐
│                      PROCESS MANAGER                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │   Python    │  │   .NET      │  │   Node.js   │             │
│  │   Process   │  │   Process   │  │   Process   │             │
│  │   Pool      │  │   Pool      │  │   Pool      │             │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘             │
│         │                │                │                     │
│         ▼                ▼                ▼                     │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                    WORKER POOL                          │   │
│  │  ┌───────┐ ┌───────┐ ┌───────┐ ┌───────┐ ┌───────┐     │   │
│  │  │ Py #1 │ │ Py #2 │ │ .NET  │ │ Node  │ │ Node  │     │   │
│  │  │       │ │       │ │  #1   │ │  #1   │ │  #2   │     │   │
│  │  └───────┘ └───────┘ └───────┘ └───────┘ └───────┘     │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  Configuration:                                                 │
│  - max_workers_per_language: 4                                  │
│  - worker_timeout_ms: 30000                                     │
│  - recycle_after_tests: 1000                                    │
│  - health_check_interval_ms: 5000                               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 7.3 Resource Limits

```yaml
resource_limits:
  per_test:
    max_duration_ms: 5000
    max_memory_mb: 512
    max_output_size_mb: 10

  per_benchmark:
    max_duration_seconds: 60
    max_memory_mb: 2048
    max_iterations: 1000000

  per_adapter:
    max_concurrent_requests: 10
    max_memory_mb: 4096
    max_cpu_percent: 80
```

---

## 8. Test & Benchmark Runner

### 8.1 Test Execution Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                    TEST EXECUTION PIPELINE                      │
└─────────────────────────────────────────────────────────────────┘

     UPS Spec                    Adapter                   Results
        │                           │                         │
        ▼                           │                         │
┌───────────────┐                   │                         │
│ Load Spec     │                   │                         │
│ & Validate    │                   │                         │
└───────┬───────┘                   │                         │
        │                           │                         │
        ▼                           │                         │
┌───────────────┐                   │                         │
│ Extract       │                   │                         │
│ Test Vectors  │                   │                         │
└───────┬───────┘                   │                         │
        │                           │                         │
        ▼                           ▼                         │
┌───────────────┐           ┌───────────────┐                 │
│ For Each Test │──────────▶│ Initialize    │                 │
│               │           │ Adapter       │                 │
└───────┬───────┘           └───────┬───────┘                 │
        │                           │                         │
        ▼                           ▼                         │
┌───────────────┐           ┌───────────────┐                 │
│ Prepare Input │──────────▶│ Execute Parse │                 │
│ (decode/load) │           │               │                 │
└───────────────┘           └───────┬───────┘                 │
                                    │                         │
                                    ▼                         │
                            ┌───────────────┐                 │
                            │ Return Result │                 │
                            │               │                 │
                            └───────┬───────┘                 │
                                    │                         │
        ┌───────────────────────────┘                         │
        │                                                     │
        ▼                                                     ▼
┌───────────────┐                                     ┌───────────────┐
│ Compare with  │────────────────────────────────────▶│ Collect       │
│ Expected      │                                     │ Results       │
└───────────────┘                                     └───────────────┘
```

### 8.2 Test Result Schema

```yaml
# TestResult Schema
test_result:
  test_id: string
  test_name: string
  category: string

  status:
    type: enum
    values: [pass, fail, skip, error, timeout]

  execution:
    started_at: datetime
    finished_at: datetime
    duration_ms: integer

  input:
    source: string  # "inline", "file", "url", "generated"
    size_bytes: integer
    encoding: string

  actual:
    success: boolean
    output: object?
    errors: Error[]?

  expected:
    success: boolean
    output: object?
    errors: Error[]?

  comparison:
    match: boolean
    diff: object?  # Structured diff if mismatch

  metadata:
    adapter: string
    library: string
    library_version: string
    options: object
```

### 8.3 Benchmark Execution Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                  BENCHMARK EXECUTION PIPELINE                   │
└─────────────────────────────────────────────────────────────────┘

┌───────────────┐
│ Load Benchmark│
│ Definition    │
└───────┬───────┘
        │
        ▼
┌───────────────┐     ┌───────────────┐
│ Prepare Input │────▶│ Generate if   │
│               │     │ needed        │
└───────┬───────┘     └───────────────┘
        │
        ▼
┌───────────────┐
│ Warmup Phase  │──── iterations: warmup_iterations
│               │     purpose: JIT, cache warming
└───────┬───────┘
        │
        ▼
┌───────────────┐
│ Measurement   │──── iterations: measurement_iterations
│ Phase         │     collect: [duration, memory, cpu]
└───────┬───────┘
        │
        ▼
┌───────────────┐
│ Statistical   │──── calculate: [mean, stddev, percentiles]
│ Analysis      │     outlier_detection: true
└───────┬───────┘
        │
        ▼
┌───────────────┐
│ Generate      │
│ Report        │
└───────────────┘
```

### 8.4 Benchmark Metrics

```yaml
benchmark_metrics:
  throughput:
    description: "Data processing rate"
    unit: "MB/s"
    calculation: "input_size_bytes / duration_seconds / 1_000_000"

  latency:
    description: "Time to process single input"
    unit: "microseconds"
    percentiles: [p50, p75, p90, p95, p99, p999]

  memory_peak:
    description: "Peak memory usage during parsing"
    unit: "MB"

  memory_allocated:
    description: "Total memory allocated"
    unit: "MB"

  cpu_time:
    description: "CPU time consumed"
    unit: "milliseconds"

  gc_collections:
    description: "Garbage collection events"
    unit: "count"
    language_specific: true
```

---

## 9. Reporting & Comparison

### 9.1 Report Types

```yaml
report_types:
  # Single library test report
  test_report:
    format: [json, html, markdown, junit-xml]
    contents:
      - summary
      - test_results
      - failure_details
      - coverage_analysis

  # Single library benchmark report
  benchmark_report:
    format: [json, html, markdown]
    contents:
      - summary
      - metrics
      - charts
      - environment_info

  # Multi-library comparison
  comparison_report:
    format: [json, html, markdown]
    contents:
      - rankings
      - side_by_side_metrics
      - recommendations
      - detailed_breakdowns

  # Certification report
  certification_report:
    format: [json, pdf]
    contents:
      - conformance_level
      - passed_requirements
      - failed_requirements
      - certification_status
```

### 9.2 Comparison Algorithm

```
┌─────────────────────────────────────────────────────────────────┐
│                    COMPARISON ALGORITHM                         │
└─────────────────────────────────────────────────────────────────┘

Input: Results from N libraries for spec S

Step 1: Conformance Scoring
┌─────────────────────────────────────────────────────────────────┐
│  conformance_score = (passed_tests / total_tests) * 100         │
│                                                                 │
│  weighted_score = Σ(test_weight[i] * pass[i]) / Σ(test_weight)  │
│                                                                 │
│  test_weights:                                                  │
│    critical: 10                                                 │
│    high: 5                                                      │
│    medium: 2                                                    │
│    low: 1                                                       │
└─────────────────────────────────────────────────────────────────┘

Step 2: Performance Scoring
┌─────────────────────────────────────────────────────────────────┐
│  # Normalize to 0-100 scale (higher is better)                  │
│  throughput_score = (library_mbps / max_mbps) * 100             │
│                                                                 │
│  # Invert for latency (lower is better)                         │
│  latency_score = (min_latency / library_latency) * 100          │
│                                                                 │
│  # Invert for memory                                            │
│  memory_score = (min_memory / library_memory) * 100             │
│                                                                 │
│  performance_score = (                                          │
│    throughput_score * 0.4 +                                     │
│    latency_score * 0.3 +                                        │
│    memory_score * 0.3                                           │
│  )                                                              │
└─────────────────────────────────────────────────────────────────┘

Step 3: Overall Scoring
┌─────────────────────────────────────────────────────────────────┐
│  # Based on UPS quality requirements weights                    │
│  overall_score = (                                              │
│    conformance_score * 0.30 +   # from quality.conformance      │
│    performance_score * 0.25 +   # from quality.performance      │
│    security_score * 0.25 +      # from quality.security         │
│    maintainability_score * 0.10 +                               │
│    community_score * 0.10                                       │
│  )                                                              │
└─────────────────────────────────────────────────────────────────┘

Output: Ranked list of libraries with scores
```

### 9.3 Output Formats

#### JSON Output
```json
{
  "spec_id": "urn:ups:parser:ietf:csv:rfc4180:1.0.0",
  "generated_at": "2025-01-15T10:30:00Z",
  "comparison": {
    "libraries_tested": 5,
    "rankings": {
      "overall": [
        {"rank": 1, "library": "Sylvan.Data.Csv", "score": 94.2},
        {"rank": 2, "library": "CsvHelper", "score": 91.5}
      ],
      "conformance": [...],
      "performance": [...]
    },
    "recommendation": {
      "best_overall": "Sylvan.Data.Csv",
      "best_for_conformance": "CsvHelper",
      "best_for_performance": "Sylvan.Data.Csv"
    }
  }
}
```

#### CLI Table Output
```
╔══════════════════════════════════════════════════════════════════╗
║           CSV Parser Comparison (RFC 4180)                       ║
╠══════════════════════════════════════════════════════════════════╣
║  RANK │ LIBRARY          │ LANG   │ CONFORM │ PERF  │ OVERALL   ║
╠══════════════════════════════════════════════════════════════════╣
║  1    │ Sylvan.Data.Csv  │ .NET   │ 97.5%   │ 890   │ ★★★★★     ║
║  2    │ CsvHelper        │ .NET   │ 98.5%   │ 450   │ ★★★★☆     ║
║  3    │ pandas           │ Python │ 95.0%   │ 380   │ ★★★★☆     ║
║  4    │ papaparse        │ Node   │ 93.0%   │ 320   │ ★★★☆☆     ║
║  5    │ csv (stdlib)     │ Python │ 90.0%   │ 150   │ ★★★☆☆     ║
╚══════════════════════════════════════════════════════════════════╝

RECOMMENDATION: Sylvan.Data.Csv for best overall performance
                CsvHelper for highest RFC 4180 conformance
```

---

## 10. Data Flow

### 10.1 Complete Execution Flow

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        COMPLETE DATA FLOW                               │
└─────────────────────────────────────────────────────────────────────────┘

User Command
     │
     ▼
┌─────────────┐
│ CLI Parser  │ ──▶ ups run csv-parser.ups.yaml --all
└──────┬──────┘
       │
       ▼
┌─────────────┐     ┌─────────────┐
│ Spec Loader │────▶│ Schema      │
│             │     │ Validator   │
└──────┬──────┘     └─────────────┘
       │
       │ Normalized Spec
       ▼
┌─────────────┐     ┌─────────────┐
│ Adapter     │────▶│ Find        │
│ Resolver    │     │ Adapters    │
└──────┬──────┘     └─────────────┘
       │
       │ [Python, .NET, Node] adapters
       ▼
┌─────────────┐     ┌─────────────────────────────────┐
│ Library     │────▶│ Discover libraries for format   │
│ Resolver    │     │ CSV: pandas, CsvHelper, etc.    │
└──────┬──────┘     └─────────────────────────────────┘
       │
       │ Library bindings
       ▼
┌─────────────┐
│ Execution   │
│ Planner     │
└──────┬──────┘
       │
       │ Execution plan
       ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         PARALLEL EXECUTION                              │
│                                                                         │
│   ┌─────────────┐    ┌─────────────┐    ┌─────────────┐                │
│   │ Python      │    │ .NET        │    │ Node.js     │                │
│   │ Worker      │    │ Worker      │    │ Worker      │                │
│   └──────┬──────┘    └──────┬──────┘    └──────┬──────┘                │
│          │                  │                  │                        │
│          ▼                  ▼                  ▼                        │
│   ┌─────────────┐    ┌─────────────┐    ┌─────────────┐                │
│   │ pandas      │    │ CsvHelper   │    │ papaparse   │                │
│   │ tests       │    │ tests       │    │ tests       │                │
│   └──────┬──────┘    └──────┬──────┘    └──────┬──────┘                │
│          │                  │                  │                        │
└──────────┼──────────────────┼──────────────────┼────────────────────────┘
           │                  │                  │
           └────────────────┬─┴──────────────────┘
                            │
                            ▼
                   ┌─────────────┐
                   │ Result      │
                   │ Collector   │
                   └──────┬──────┘
                          │
                          ▼
                   ┌─────────────┐
                   │ Comparison  │
                   │ Engine      │
                   └──────┬──────┘
                          │
                          ▼
                   ┌─────────────┐
                   │ Report      │
                   │ Generator   │
                   └──────┬──────┘
                          │
                          ▼
                   ┌─────────────┐
                   │   Output    │
                   │ (CLI/File)  │
                   └─────────────┘
```

### 10.2 IPC Message Flow

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         IPC MESSAGE FLOW                                │
└─────────────────────────────────────────────────────────────────────────┘

UPS Runtime                                              Python Adapter
     │                                                         │
     │  ────────────────────────────────────────────────────▶  │
     │  { "method": "initialize",                              │
     │    "params": { "library": "pandas" } }                  │
     │                                                         │
     │  ◀────────────────────────────────────────────────────  │
     │  { "result": { "success": true,                         │
     │                "capabilities": ["parse", "serialize"] }}│
     │                                                         │
     │  ────────────────────────────────────────────────────▶  │
     │  { "method": "parse",                                   │
     │    "params": { "data": "a,b,c\n1,2,3",                  │
     │                "options": { "has_header": true } } }    │
     │                                                         │
     │  ◀────────────────────────────────────────────────────  │
     │  { "result": { "success": true,                         │
     │                "output": { "headers": ["a","b","c"],    │
     │                            "records": [["1","2","3"]] },│
     │                "duration_ns": 125000,                   │
     │                "memory_bytes": 1024 } }                 │
     │                                                         │
     ▼                                                         ▼
```

---

## 11. CLI Interface

### 11.1 Command Structure

```bash
ups-runtime <command> [subcommand] [options]

Commands:
  test        Run conformance tests
  benchmark   Run performance benchmarks
  compare     Compare multiple libraries
  list        List available adapters/libraries
  validate    Validate UPS spec file
  info        Show spec/library information
  init        Initialize new adapter project
```

### 11.2 Command Examples

```bash
# Run all tests for a spec using all available adapters
ups-runtime test specs/csv-parser.ups.yaml --all

# Run tests with specific adapter and library
ups-runtime test specs/csv-parser.ups.yaml \
  --adapter python \
  --library pandas

# Run only specific test categories
ups-runtime test specs/csv-parser.ups.yaml \
  --adapter dotnet \
  --library CsvHelper \
  --categories valid,invalid,edge-case

# Run benchmarks
ups-runtime benchmark specs/csv-parser.ups.yaml \
  --adapter python \
  --library pandas \
  --iterations 1000 \
  --warmup 100

# Compare multiple libraries
ups-runtime compare specs/csv-parser.ups.yaml \
  --libraries "python:pandas,dotnet:CsvHelper,node:papaparse" \
  --output comparison.json \
  --format json

# List available libraries for a format
ups-runtime list libraries --format csv

# List installed adapters
ups-runtime list adapters

# Validate a UPS spec
ups-runtime validate specs/my-parser.ups.yaml

# Show detailed info about a library
ups-runtime info --library dotnet:CsvHelper

# Initialize a new adapter project
ups-runtime init adapter --language rust --name rust-adapter
```

### 11.3 Configuration File

```yaml
# ~/.ups-runtime/config.yaml
defaults:
  output_format: table
  parallel_workers: 4
  timeout_ms: 30000

adapters:
  python:
    path: ~/.ups-runtime/adapters/python
    python_executable: python3

  dotnet:
    path: ~/.ups-runtime/adapters/dotnet
    dotnet_executable: dotnet

  node:
    path: ~/.ups-runtime/adapters/node
    node_executable: node

cache:
  enabled: true
  directory: ~/.ups-runtime/cache
  max_size_mb: 1024
  ttl_hours: 24

reporting:
  default_format: table
  save_results: true
  results_directory: ~/.ups-runtime/results
```

---

## 12. Extension Points

### 12.1 Custom Adapter Development

```yaml
# Adapter SDK provides:
adapter_sdk:
  interfaces:
    - UniversalParserAdapter  # Main interface
    - LibraryBinding          # Per-library binding
    - MetricsCollector        # Performance metrics

  utilities:
    - JsonRpcServer           # IPC handling
    - OutputNormalizer        # Normalize to UPS output schema
    - ErrorMapper             # Map library errors to UPS errors

  templates:
    - Python adapter template
    - .NET adapter template
    - Node.js adapter template
    - Rust adapter template
```

### 12.2 Custom Report Generators

```yaml
# Report Generator Plugin Interface
interface: ReportGenerator

methods:
  generate:
    input:
      results: ComparisonResults
      options: ReportOptions
    output:
      report: bytes  # Generated report content

  get_supported_formats:
    output:
      formats: string[]  # ["html", "pdf", "csv"]
```

### 12.3 Custom Metrics

```yaml
# Custom Metric Plugin Interface
interface: MetricCollector

methods:
  collect:
    input:
      execution_context: ExecutionContext
    output:
      metrics: Metric[]

  get_metric_definitions:
    output:
      definitions: MetricDefinition[]
```

---

## 13. Security Considerations

### 13.1 Sandboxing

```yaml
sandboxing:
  # Adapter process isolation
  process_isolation:
    enabled: true
    capabilities:
      - no_network        # Disable network access
      - readonly_fs       # Read-only filesystem (except temp)
      - limited_memory    # Memory cgroup limits
      - limited_cpu       # CPU cgroup limits

  # Container isolation (when using container mode)
  container_isolation:
    enabled: true
    image: ups-runtime-sandbox:latest
    seccomp_profile: strict
    no_new_privileges: true

  # Input validation
  input_validation:
    max_input_size_mb: 100
    allowed_encodings: [utf-8, utf-16, ascii, iso-8859-1]
    sanitize_file_paths: true
```

### 13.2 Trust Model

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           TRUST MODEL                                   │
└─────────────────────────────────────────────────────────────────────────┘

┌─────────────────────┐
│   TRUSTED           │
│   ─────────         │
│   • UPS Specs       │  ← Validated against schema
│   • Core Runtime    │  ← Signed releases
│   • Official        │  ← Maintained by UPP team
│     Adapters        │
└─────────────────────┘

┌─────────────────────┐
│   SEMI-TRUSTED      │
│   ────────────      │
│   • Community       │  ← Code reviewed
│     Adapters        │
│   • Library         │  ← From official package managers
│     Bindings        │
└─────────────────────┘

┌─────────────────────┐
│   UNTRUSTED         │
│   ─────────         │
│   • Test Inputs     │  ← May contain malicious data
│   • External        │  ← Fetched at runtime
│     Resources       │
└─────────────────────┘
```

---

## 14. Implementation Phases

### Phase 1: Foundation (MVP)

```yaml
phase_1:
  name: "Foundation"
  duration: "4-6 weeks"

  deliverables:
    - UPS Spec Loader (YAML/JSON)
    - Schema Validator
    - Basic Test Runner
    - Python Adapter (CSV only)
    - CLI with test command
    - JSON output

  scope:
    formats: [csv]
    adapters: [python]
    libraries: [csv-stdlib, pandas]
    features:
      - Run test_vectors
      - Pass/fail reporting
      - Basic metrics
```

### Phase 2: Multi-Language

```yaml
phase_2:
  name: "Multi-Language Support"
  duration: "4-6 weeks"

  deliverables:
    - .NET Adapter
    - Node.js Adapter
    - Process Manager
    - IPC Bridge
    - Parallel execution

  scope:
    formats: [csv, json]
    adapters: [python, dotnet, node]
    libraries: [pandas, CsvHelper, papaparse, Newtonsoft.Json]
    features:
      - Cross-language testing
      - Comparison reports
```

### Phase 3: Benchmarking

```yaml
phase_3:
  name: "Performance Benchmarking"
  duration: "3-4 weeks"

  deliverables:
    - Benchmark Runner
    - Statistical analysis
    - Performance metrics
    - Memory profiling

  scope:
    features:
      - Throughput measurement
      - Latency percentiles
      - Memory tracking
      - Comparison charts
```

### Phase 4: Production Ready

```yaml
phase_4:
  name: "Production Ready"
  duration: "4-6 weeks"

  deliverables:
    - Container support
    - CI/CD integration
    - HTML reports
    - Caching system
    - Plugin system

  scope:
    features:
      - Docker execution mode
      - GitHub Actions integration
      - Beautiful HTML reports
      - Result caching
      - Custom adapter support
```

---

## Appendix A: File Structure

```
ups-runtime/
├── src/
│   ├── core/
│   │   ├── spec_loader.py
│   │   ├── schema_validator.py
│   │   ├── test_executor.py
│   │   ├── benchmark_runner.py
│   │   ├── comparison_engine.py
│   │   └── report_generator.py
│   ├── adapters/
│   │   ├── manager.py
│   │   ├── process_pool.py
│   │   └── ipc_bridge.py
│   ├── cli/
│   │   ├── main.py
│   │   ├── commands/
│   │   │   ├── test.py
│   │   │   ├── benchmark.py
│   │   │   ├── compare.py
│   │   │   └── list.py
│   │   └── formatters/
│   │       ├── table.py
│   │       ├── json.py
│   │       └── html.py
│   └── utils/
│       ├── config.py
│       ├── cache.py
│       └── metrics.py
├── adapters/
│   ├── python/
│   │   ├── ups_adapter/
│   │   │   ├── main.py
│   │   │   └── bindings/
│   │   │       ├── csv_stdlib.py
│   │   │       └── pandas_csv.py
│   │   └── pyproject.toml
│   ├── dotnet/
│   │   └── UpsAdapter/
│   └── node/
│       └── ups-adapter/
├── specs/
│   └── examples/
├── tests/
├── docs/
└── pyproject.toml
```

---

## Appendix B: Glossary

| Term | Definition |
|------|------------|
| **UPS** | Universal Parser Specification - the standard format for defining parsers |
| **Adapter** | Language-specific component that bridges UPS Runtime and libraries |
| **Binding** | Library-specific implementation within an adapter |
| **Conformance** | Degree to which a library follows the specification |
| **Test Vector** | Single test case with input and expected output |
| **IPC** | Inter-Process Communication between runtime and adapters |

---

*End of UPS Runtime Architecture Document*
