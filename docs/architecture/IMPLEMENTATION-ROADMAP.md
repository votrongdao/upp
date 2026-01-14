# UPS Runtime Implementation Roadmap

## From Specification to Working Software

**Version:** 1.0.0
**Last Updated:** January 2025

---

## Executive Summary

This roadmap outlines the practical implementation path from the UPS specification to a working runtime system. Following the **MVP-first** philosophy, we start with the simplest possible implementation that delivers value, then iterate.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    IMPLEMENTATION PHILOSOPHY                            │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│   "Make it work, make it right, make it fast"                          │
│                                                                         │
│   Phase 0: Make it work  → Single format, single adapter               │
│   Phase 1: Make it right → Multiple formats, multiple adapters         │
│   Phase 2: Make it fast  → Benchmarking, optimization                  │
│   Phase 3: Make it scale → Production features, ecosystem              │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Phase 0: Proof of Concept (Week 1-2)

### Goal
Validate the core architecture with minimal implementation.

### Scope
```yaml
formats: [csv]              # Only CSV
adapters: [python]          # Only Python
libraries: [csv-stdlib]     # Only stdlib csv
features: [test]            # Only test command
output: [json]              # Only JSON output
```

### Deliverables

#### 0.1 Spec Loader
```python
# Simple YAML loader - no fancy features yet
def load_spec(path: str) -> dict:
    with open(path) as f:
        return yaml.safe_load(f)
```

#### 0.2 Test Runner (Minimal)
```python
def run_tests(spec: dict, adapter) -> list:
    results = []
    for test in spec["conformance"]["test_vectors"]:
        input_data = test["input"]["inline"]
        expected = test["expected"]

        actual = adapter.parse(input_data)
        passed = compare(actual, expected)

        results.append({
            "test_id": test["id"],
            "passed": passed
        })
    return results
```

#### 0.3 Python Adapter (Inline)
```python
# No IPC yet - direct function calls
class CsvStdlibAdapter:
    def parse(self, data: str, options: dict) -> dict:
        import csv
        from io import StringIO

        reader = csv.reader(StringIO(data))
        rows = list(reader)

        return {
            "headers": rows[0] if options.get("has_header") else None,
            "records": rows[1:] if options.get("has_header") else rows,
            "rowCount": len(rows) - (1 if options.get("has_header") else 0)
        }
```

#### 0.4 CLI (Minimal)
```bash
# Week 1 goal: this command works
ups test specs/examples/csv-parser.ups.yaml

# Output: JSON with test results
{"passed": 5, "failed": 1, "results": [...]}
```

### File Structure (Phase 0)
```
ups-runtime/
├── ups_runtime/
│   ├── __init__.py
│   ├── cli.py              # argparse, minimal commands
│   ├── spec_loader.py      # YAML loading
│   ├── test_runner.py      # Test execution
│   └── adapters/
│       └── python_csv.py   # Direct Python CSV adapter
├── pyproject.toml
└── README.md
```

### Success Criteria
- [ ] `ups test csv-parser.ups.yaml` runs without error
- [ ] Outputs JSON with pass/fail for each test
- [ ] At least 80% tests pass with Python csv stdlib

---

## Phase 1: Foundation (Week 3-6)

### Goal
Proper architecture with process isolation and multiple libraries.

### Scope
```yaml
formats: [csv]
adapters: [python]
libraries: [csv-stdlib, pandas]
features: [test, validate]
output: [json, table]
```

### Deliverables

#### 1.1 JSON-RPC Adapter Protocol
Implement the full adapter protocol from ADAPTER-SPECIFICATION.md:

```python
# Adapter runs as separate process
# Communication via stdin/stdout JSON-RPC
```

#### 1.2 Process Manager
```python
class ProcessManager:
    def spawn_adapter(self, adapter_type: str) -> AdapterProcess:
        """Spawn adapter as subprocess."""
        pass

    def send_request(self, process, method: str, params: dict) -> dict:
        """Send JSON-RPC request to adapter."""
        pass
```

#### 1.3 Multiple Library Support
```python
# bindings/pandas_csv.py
class PandasCsvBinding:
    def parse(self, data: bytes, options: dict) -> dict:
        import pandas as pd
        df = pd.read_csv(BytesIO(data), **self._map_options(options))
        return self._normalize_output(df)
```

#### 1.4 Schema Validation
```python
def validate_spec(spec: dict, schema_path: str) -> ValidationResult:
    """Validate spec against UPS JSON Schema."""
    with open(schema_path) as f:
        schema = json.load(f)
    jsonschema.validate(spec, schema)
```

#### 1.5 Table Output Formatter
```python
def format_table(results: list) -> str:
    """Pretty table output for terminal."""
    pass
```

### File Structure (Phase 1)
```
ups-runtime/
├── ups_runtime/
│   ├── __init__.py
│   ├── cli/
│   │   ├── __init__.py
│   │   ├── main.py
│   │   ├── test_command.py
│   │   └── formatters/
│   │       ├── json_formatter.py
│   │       └── table_formatter.py
│   ├── core/
│   │   ├── spec_loader.py
│   │   ├── schema_validator.py
│   │   └── test_runner.py
│   └── adapters/
│       ├── manager.py
│       ├── process.py
│       └── ipc.py
├── adapters/
│   └── python/
│       ├── ups_adapter/
│       │   ├── main.py
│       │   ├── interface.py
│       │   └── bindings/
│       │       ├── csv_stdlib.py
│       │       └── pandas_csv.py
│       └── pyproject.toml
├── pyproject.toml
└── README.md
```

### Success Criteria
- [ ] Adapter runs as separate process
- [ ] JSON-RPC communication working
- [ ] Both csv-stdlib and pandas bindings work
- [ ] Table output looks good
- [ ] `ups validate` command works

---

## Phase 2: Multi-Language (Week 7-10)

### Goal
Support .NET and Node.js adapters.

### Scope
```yaml
formats: [csv, json]
adapters: [python, dotnet, node]
libraries:
  csv: [csv-stdlib, pandas, CsvHelper, papaparse]
  json: [json-stdlib, Newtonsoft.Json]
features: [test, validate, list]
output: [json, table, markdown]
```

### Deliverables

#### 2.1 .NET Adapter
```csharp
// adapters/dotnet/UpsAdapter/Program.cs
public class Program
{
    public static void Main()
    {
        var server = new JsonRpcServer(new AdapterInterface());
        server.Run();
    }
}
```

#### 2.2 Node.js Adapter
```javascript
// adapters/node/ups-adapter/main.js
const readline = require('readline');
const adapter = require('./interface');

const rl = readline.createInterface({ input: process.stdin });

rl.on('line', (line) => {
    const request = JSON.parse(line);
    const response = adapter.handleRequest(request);
    console.log(JSON.stringify(response));
});
```

#### 2.3 Library Registry
```yaml
# library-registry.yaml
libraries:
  - id: python:pandas
    adapter: python
    package: pandas
    formats: [csv, json, excel]

  - id: dotnet:csvhelper
    adapter: dotnet
    package: CsvHelper
    formats: [csv]

  - id: node:papaparse
    adapter: node
    package: papaparse
    formats: [csv]
```

#### 2.4 JSON Parser Support
Add json-parser.ups.yaml test support.

#### 2.5 Compare Command (Basic)
```bash
ups compare csv-parser.ups.yaml --all
```

### Success Criteria
- [ ] .NET adapter working with CsvHelper
- [ ] Node adapter working with papaparse
- [ ] Cross-language comparison working
- [ ] `ups list libraries --format csv` shows all

---

## Phase 3: Benchmarking (Week 11-14)

### Goal
Add performance measurement capabilities.

### Scope
```yaml
features: [test, validate, list, benchmark, compare]
metrics: [throughput, latency, memory]
```

### Deliverables

#### 3.1 Benchmark Runner
```python
class BenchmarkRunner:
    def run(self, spec: dict, adapter, iterations: int) -> BenchmarkResult:
        """Run benchmark with statistical analysis."""
        results = []

        # Warmup
        for _ in range(self.warmup_iterations):
            adapter.parse(input_data)

        # Measurement
        for _ in range(iterations):
            metrics = adapter.parse_with_metrics(input_data)
            results.append(metrics)

        return self._analyze(results)
```

#### 3.2 Metrics Collection in Adapters
```python
def parse_with_metrics(self, data: bytes, options: dict) -> dict:
    tracemalloc.start()
    start = time.perf_counter_ns()

    result = self._parse(data, options)

    duration_ns = time.perf_counter_ns() - start
    current, peak = tracemalloc.get_traced_memory()
    tracemalloc.stop()

    return {
        "result": result,
        "metrics": {
            "duration_ns": duration_ns,
            "memory_bytes": current,
            "peak_memory_bytes": peak
        }
    }
```

#### 3.3 Statistical Analysis
```python
def analyze_benchmarks(results: list) -> dict:
    durations = [r["duration_ns"] for r in results]
    return {
        "mean": statistics.mean(durations),
        "stddev": statistics.stdev(durations),
        "p50": percentile(durations, 50),
        "p99": percentile(durations, 99),
        "throughput_mbps": calculate_throughput(input_size, durations)
    }
```

#### 3.4 Comparison Engine
```python
class ComparisonEngine:
    def compare(self, results: dict) -> ComparisonReport:
        """Compare results across libraries."""
        rankings = self._calculate_rankings(results)
        recommendations = self._generate_recommendations(rankings)
        return ComparisonReport(rankings, recommendations)
```

### Success Criteria
- [ ] `ups benchmark` produces consistent results
- [ ] Memory tracking working across all adapters
- [ ] Statistical analysis (mean, stddev, percentiles)
- [ ] `ups compare` generates rankings

---

## Phase 4: Production Ready (Week 15-20)

### Goal
Polish for production use.

### Deliverables

#### 4.1 HTML Reports
```bash
ups compare csv-parser.ups.yaml --all --format html -o report.html
```

#### 4.2 JUnit XML Output
```bash
ups test csv-parser.ups.yaml --format junit -o results.xml
```

#### 4.3 CI/CD Integration
- GitHub Actions workflow template
- GitLab CI template
- Azure DevOps template

#### 4.4 Docker Support
```dockerfile
FROM python:3.11-slim
RUN pip install ups-runtime
ENTRYPOINT ["ups"]
```

#### 4.5 Documentation
- Getting Started Guide
- API Reference
- Adapter Development Guide

#### 4.6 Package Publishing
- PyPI: `ups-runtime`
- npm: `@upp/runtime`
- NuGet: `UpsRuntime`

### Success Criteria
- [ ] Published to PyPI
- [ ] Docker image available
- [ ] CI/CD templates working
- [ ] Documentation complete

---

## Technical Decisions

### Language Choice: Python for Core

**Rationale:**
- Fast prototyping
- Rich ecosystem (YAML, JSON Schema, statistics)
- Good subprocess management
- Easy distribution (pip)
- Most users have Python

### IPC: JSON-RPC over stdio

**Rationale:**
- Language agnostic
- Simple to implement
- Debuggable (text format)
- No network dependencies
- Works in containers

### Adapter Model: Subprocess

**Rationale:**
- Process isolation (crash safety)
- Memory limits possible
- Language independence
- No FFI complexity

---

## Risk Mitigation

| Risk | Mitigation |
|------|------------|
| Adapter crashes | Process isolation, restart logic |
| Slow IPC | Batch operations, streaming for large inputs |
| Library incompatibility | Version pinning, compatibility matrix |
| Platform differences | CI testing on Win/Mac/Linux |
| Memory leaks | Per-test process recycling |

---

## MVP Checklist

### Week 2 Deliverable
```bash
# This must work:
pip install ups-runtime
ups test specs/csv-parser.ups.yaml -l python:csv
```

### Week 6 Deliverable
```bash
# This must work:
ups test specs/csv-parser.ups.yaml -l python:pandas
ups test specs/csv-parser.ups.yaml --all  # python:csv + python:pandas
ups validate specs/csv-parser.ups.yaml
```

### Week 10 Deliverable
```bash
# This must work:
ups test specs/csv-parser.ups.yaml --all  # Python + .NET + Node
ups list libraries --format csv
ups compare specs/csv-parser.ups.yaml --all
```

---

## Directory Structure (Final)

```
upp/
├── docs/
│   ├── specification/
│   │   └── UPS-SPECIFICATION-v1.0.md
│   ├── architecture/
│   │   ├── UPS-RUNTIME-ARCHITECTURE.md
│   │   ├── ADAPTER-SPECIFICATION.md
│   │   ├── CLI-DESIGN.md
│   │   └── IMPLEMENTATION-ROADMAP.md
│   └── guides/
│       └── GETTING-STARTED.md
│
├── specs/
│   ├── schema/
│   │   └── ups-schema-v1.0.json
│   └── examples/
│       ├── csv-parser.ups.yaml
│       ├── json-parser.ups.yaml
│       └── xml-parser.ups.yaml
│
├── runtime/                    # UPS Runtime (Python)
│   ├── ups_runtime/
│   │   ├── cli/
│   │   ├── core/
│   │   └── adapters/
│   ├── tests/
│   └── pyproject.toml
│
├── adapters/
│   ├── python/                 # Python adapter
│   │   ├── ups_adapter/
│   │   └── pyproject.toml
│   ├── dotnet/                 # .NET adapter
│   │   └── UpsAdapter/
│   └── node/                   # Node.js adapter
│       └── ups-adapter/
│
└── README.md
```

---

## Getting Started (For Developers)

### Setup Development Environment

```bash
# Clone repository
git clone https://github.com/universalparser/upp.git
cd upp

# Create virtual environment
python -m venv .venv
source .venv/bin/activate

# Install in development mode
cd runtime
pip install -e ".[dev]"

# Run tests
pytest

# Try the CLI
ups --help
ups test ../specs/examples/csv-parser.ups.yaml -l python:csv
```

### Implement New Adapter

1. Copy `adapters/python/` as template
2. Implement JSON-RPC server
3. Implement `LibraryBinding` for target library
4. Register in library registry
5. Test with `ups test`

### Add New Library Binding

1. Create binding file in `adapters/<lang>/bindings/`
2. Implement `parse()`, `serialize()` methods
3. Map library options to UPS options
4. Normalize output to UPS schema
5. Add to adapter's library list

---

## Success Metrics

| Metric | Week 2 | Week 6 | Week 14 | Week 20 |
|--------|--------|--------|---------|---------|
| Formats supported | 1 | 1 | 2 | 4+ |
| Adapters | 1 | 1 | 3 | 3+ |
| Libraries | 1 | 2 | 6 | 10+ |
| Test coverage | 60% | 80% | 85% | 90% |
| PyPI downloads | - | - | 100+ | 1000+ |

---

*End of Implementation Roadmap*
