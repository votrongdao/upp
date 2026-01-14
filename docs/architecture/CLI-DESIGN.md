# UPS Runtime CLI Design

## Command Line Interface Specification

**Version:** 1.0.0
**Status:** Draft
**Last Updated:** January 2025

---

## 1. Overview

The UPS Runtime CLI (`ups`) is the primary interface for running conformance tests, benchmarks, and comparisons against parser implementations.

### Design Principles

1. **Simple by Default**: Common operations should require minimal flags
2. **Progressive Disclosure**: Advanced options available but not required
3. **Composable**: Commands can be piped and scripted
4. **Cross-Platform**: Works on Windows, macOS, Linux

---

## 2. Installation

```bash
# Via pip (Python)
pip install ups-runtime

# Via npm (Node.js wrapper)
npm install -g @upp/runtime

# Via dotnet tool
dotnet tool install -g UpsRuntime

# Via Homebrew (macOS/Linux)
brew install upp/tap/ups-runtime

# Direct download
curl -fsSL https://get.universalparser.org | bash
```

---

## 3. Command Structure

```
ups <command> [subcommand] [arguments] [options]

Global Options:
  -h, --help           Show help
  -v, --version        Show version
  -q, --quiet          Suppress output
  --verbose            Verbose output
  --no-color           Disable colored output
  --config <path>      Use custom config file
  --json               Output as JSON
```

---

## 4. Commands

### 4.1 `ups test` - Run Conformance Tests

```bash
ups test <spec-file> [options]

Arguments:
  spec-file            UPS specification file (.ups.yaml or .ups.json)

Options:
  -a, --adapter <name>     Language adapter to use (python, dotnet, node)
  -l, --library <id>       Specific library (e.g., python:pandas)
  --all                    Run with all available adapters/libraries
  -c, --categories <list>  Test categories: valid,invalid,edge-case,security
  -t, --tags <list>        Filter by tags
  --filter <pattern>       Filter test IDs by pattern
  --timeout <ms>           Per-test timeout (default: 5000)
  --parallel <n>           Parallel test execution (default: CPU count)
  -o, --output <file>      Write results to file
  --format <fmt>           Output format: table, json, junit, markdown
  --fail-fast              Stop on first failure
  --no-metrics             Skip metrics collection

Examples:
  # Run all tests with pandas
  ups test specs/csv-parser.ups.yaml -l python:pandas

  # Run with all available CSV libraries
  ups test specs/csv-parser.ups.yaml --all

  # Run only 'valid' and 'edge-case' tests
  ups test specs/csv-parser.ups.yaml -l python:pandas -c valid,edge-case

  # Output JUnit XML for CI
  ups test specs/csv-parser.ups.yaml --all --format junit -o results.xml

  # Filter specific tests
  ups test specs/csv-parser.ups.yaml -l python:pandas --filter "quoted-*"
```

**Output Example (Table):**
```
╔═══════════════════════════════════════════════════════════════════════════╗
║  CSV Parser Conformance Tests                                             ║
║  Library: python:pandas (v2.1.0)                                          ║
╠═══════════════════════════════════════════════════════════════════════════╣
║  STATUS │ TEST ID          │ NAME                     │ TIME    │ MEMORY  ║
╠═══════════════════════════════════════════════════════════════════════════╣
║  ✓ PASS │ simple-csv       │ Simple CSV               │ 1.2ms   │ 2.1KB   ║
║  ✓ PASS │ quoted-fields    │ Quoted fields with commas│ 1.5ms   │ 2.3KB   ║
║  ✓ PASS │ embedded-quotes  │ Embedded quotes          │ 1.3ms   │ 2.1KB   ║
║  ✓ PASS │ multiline-field  │ Multiline field          │ 1.8ms   │ 2.5KB   ║
║  ✓ PASS │ empty-fields     │ Empty fields             │ 1.1ms   │ 2.0KB   ║
║  ✗ FAIL │ unclosed-quote   │ Unclosed quote           │ 0.8ms   │ 1.5KB   ║
╠═══════════════════════════════════════════════════════════════════════════╣
║  SUMMARY: 5/6 passed (83.3%)  │  Total: 7.7ms  │  Peak Memory: 2.5KB      ║
╚═══════════════════════════════════════════════════════════════════════════╝

FAILED TESTS:
  unclosed-quote:
    Expected: error with code CSV001
    Actual:   success (pandas accepted malformed input)
```

---

### 4.2 `ups benchmark` - Run Performance Benchmarks

```bash
ups benchmark <spec-file> [options]

Options:
  -a, --adapter <name>     Language adapter
  -l, --library <id>       Specific library
  --all                    Run with all available libraries
  -i, --iterations <n>     Measurement iterations (default: 1000)
  -w, --warmup <n>         Warmup iterations (default: 100)
  --timeout <seconds>      Max time per benchmark (default: 60)
  --input-size <size>      Override input size (e.g., 1MB, 100MB)
  -o, --output <file>      Write results to file
  --format <fmt>           Output format: table, json, csv, markdown

Examples:
  # Benchmark pandas CSV parsing
  ups benchmark specs/csv-parser.ups.yaml -l python:pandas

  # Compare all CSV libraries
  ups benchmark specs/csv-parser.ups.yaml --all

  # Quick benchmark with fewer iterations
  ups benchmark specs/csv-parser.ups.yaml --all -i 100 -w 10

  # Benchmark with large input
  ups benchmark specs/csv-parser.ups.yaml -l python:pandas --input-size 100MB
```

**Output Example:**
```
╔═══════════════════════════════════════════════════════════════════════════╗
║  CSV Parser Benchmark Results                                             ║
║  Input: 10MB CSV (100,000 rows × 20 columns)                              ║
╠═══════════════════════════════════════════════════════════════════════════╣
║  LIBRARY          │ THROUGHPUT │ LATENCY (p99) │ MEMORY  │ SCORE         ║
╠═══════════════════════════════════════════════════════════════════════════╣
║  dotnet:sylvan    │ 890 MB/s   │ 12.3ms        │ 45MB    │ ★★★★★ (95.2) ║
║  dotnet:csvhelper │ 450 MB/s   │ 23.1ms        │ 128MB   │ ★★★★☆ (78.5) ║
║  python:polars    │ 720 MB/s   │ 15.2ms        │ 89MB    │ ★★★★★ (89.1) ║
║  python:pandas    │ 380 MB/s   │ 28.5ms        │ 256MB   │ ★★★☆☆ (65.3) ║
║  node:papaparse   │ 320 MB/s   │ 35.2ms        │ 178MB   │ ★★★☆☆ (58.7) ║
║  python:csv       │ 150 MB/s   │ 72.1ms        │ 312MB   │ ★★☆☆☆ (42.1) ║
╚═══════════════════════════════════════════════════════════════════════════╝

WINNER: dotnet:sylvan (Sylvan.Data.Csv)
  - 2.3x faster than pandas
  - 5.7x less memory than pandas
  - Meets UPS quality.performance.target_mbps (500 MB/s)
```

---

### 4.3 `ups compare` - Compare Libraries

```bash
ups compare <spec-file> [options]

Options:
  --libraries <list>       Libraries to compare (comma-separated)
  --all                    Compare all available libraries
  --weights <json>         Custom scoring weights
  -o, --output <file>      Write report to file
  --format <fmt>           Output format: table, json, html, markdown

Examples:
  # Compare specific libraries
  ups compare specs/csv-parser.ups.yaml \
    --libraries "python:pandas,dotnet:csvhelper,node:papaparse"

  # Compare all with custom weights
  ups compare specs/csv-parser.ups.yaml --all \
    --weights '{"conformance":0.5,"performance":0.3,"memory":0.2}'

  # Generate HTML report
  ups compare specs/csv-parser.ups.yaml --all --format html -o report.html
```

**Output Example:**
```
╔═══════════════════════════════════════════════════════════════════════════╗
║                    CSV PARSER COMPARISON REPORT                           ║
║                    Format: RFC 4180 (csv-parser.ups.yaml)                 ║
╠═══════════════════════════════════════════════════════════════════════════╣

OVERALL RANKING
───────────────
  #1  Sylvan.Data.Csv   (.NET)    ████████████████████ 94.2
  #2  CsvHelper         (.NET)    █████████████████░░░ 87.5
  #3  polars            (Python)  ████████████████░░░░ 82.3
  #4  pandas            (Python)  ██████████████░░░░░░ 71.8
  #5  papaparse         (Node.js) █████████████░░░░░░░ 68.4
  #6  csv (stdlib)      (Python)  ██████████░░░░░░░░░░ 52.1

CATEGORY WINNERS
────────────────
  🎯 Best Conformance:  CsvHelper         (98.5% tests passed)
  ⚡ Best Performance:  Sylvan.Data.Csv   (890 MB/s)
  💾 Best Memory:       Sylvan.Data.Csv   (45 MB peak)
  🔒 Best Security:     CsvHelper         (all protections)

DETAILED SCORES
───────────────
  Library          │ Conform │ Perf  │ Memory │ Security │ Overall
  ─────────────────┼─────────┼───────┼────────┼──────────┼─────────
  Sylvan.Data.Csv  │ 97.5    │ 100.0 │ 100.0  │ 85.0     │ 94.2
  CsvHelper        │ 98.5    │ 65.2  │ 78.5   │ 100.0    │ 87.5
  polars           │ 92.0    │ 85.5  │ 82.1   │ 75.0     │ 82.3
  pandas           │ 95.0    │ 52.3  │ 45.2   │ 80.0     │ 71.8
  papaparse        │ 88.5    │ 45.8  │ 52.3   │ 72.0     │ 68.4
  csv (stdlib)     │ 85.0    │ 25.2  │ 35.1   │ 70.0     │ 52.1

RECOMMENDATIONS
───────────────
  ✓ For PRODUCTION use:     Sylvan.Data.Csv (best overall)
  ✓ For STRICT conformance: CsvHelper (highest RFC compliance)
  ✓ For PYTHON projects:    polars (best Python performance)
  ✓ For MEMORY-constrained: Sylvan.Data.Csv (lowest memory)

╚═══════════════════════════════════════════════════════════════════════════╝
```

---

### 4.4 `ups list` - List Available Resources

```bash
ups list <resource> [options]

Resources:
  adapters         List installed language adapters
  libraries        List available libraries
  formats          List supported parser formats
  specs            List available UPS specifications

Options:
  --format <id>    Filter by format (e.g., csv, json)
  --adapter <name> Filter by adapter
  --installed      Show only installed
  --available      Show available for installation

Examples:
  # List all adapters
  ups list adapters

  # List CSV libraries
  ups list libraries --format csv

  # List all available specs
  ups list specs
```

**Output Example:**
```
╔═══════════════════════════════════════════════════════════════════════════╗
║  Available Libraries for Format: CSV                                      ║
╠═══════════════════════════════════════════════════════════════════════════╣
║  ID               │ NAME           │ ADAPTER │ VERSION │ STATUS          ║
╠═══════════════════════════════════════════════════════════════════════════╣
║  python:csv       │ csv (stdlib)   │ python  │ builtin │ ✓ installed     ║
║  python:pandas    │ pandas         │ python  │ 2.1.0   │ ✓ installed     ║
║  python:polars    │ polars         │ python  │ 0.19.0  │ ○ available     ║
║  dotnet:csvhelper │ CsvHelper      │ dotnet  │ 30.0.1  │ ✓ installed     ║
║  dotnet:sylvan    │ Sylvan.Data    │ dotnet  │ 1.3.5   │ ○ available     ║
║  node:papaparse   │ Papa Parse     │ node    │ 5.4.1   │ ✓ installed     ║
║  node:csv-parse   │ csv-parse      │ node    │ 5.5.0   │ ○ available     ║
╚═══════════════════════════════════════════════════════════════════════════╝

To install: ups install <library-id>
```

---

### 4.5 `ups validate` - Validate UPS Specification

```bash
ups validate <spec-file> [options]

Options:
  --strict           Enable strict validation
  --check-references Verify external references are accessible
  --schema <path>    Use custom schema file

Examples:
  # Basic validation
  ups validate my-parser.ups.yaml

  # Strict validation with reference checking
  ups validate my-parser.ups.yaml --strict --check-references
```

**Output Example:**
```
✓ Schema validation passed
✓ All required fields present
✓ Grammar syntax valid (ABNF)
✓ Test vectors well-formed (15 tests)
⚠ Warning: No security tests defined
⚠ Warning: Missing performance benchmarks

Result: VALID (with 2 warnings)
```

---

### 4.6 `ups info` - Show Information

```bash
ups info [resource] [options]

Resources:
  spec <file>        Show spec details
  library <id>       Show library details
  adapter <name>     Show adapter details

Examples:
  # Show spec info
  ups info spec specs/csv-parser.ups.yaml

  # Show library info
  ups info library python:pandas
```

---

### 4.7 `ups init` - Initialize New Projects

```bash
ups init <type> [options]

Types:
  spec              Create new UPS specification
  adapter           Create new language adapter project

Options:
  --format <id>     Parser format (for spec)
  --language <lang> Target language (for adapter)
  --name <name>     Project name
  --output <dir>    Output directory

Examples:
  # Create new CSV parser spec
  ups init spec --format csv --name my-csv-parser

  # Create Python adapter project
  ups init adapter --language python --name my-adapter
```

---

## 5. Configuration

### 5.1 Config File Location

```
~/.ups/config.yaml           # User config
./.ups/config.yaml           # Project config (takes precedence)
```

### 5.2 Config Schema

```yaml
# ~/.ups/config.yaml
version: 1

# Default output settings
output:
  format: table              # table, json, markdown
  color: auto                # auto, always, never
  verbose: false

# Adapter settings
adapters:
  python:
    enabled: true
    executable: python3
    env:
      PYTHONPATH: /custom/path

  dotnet:
    enabled: true
    executable: dotnet

  node:
    enabled: true
    executable: node

# Test settings
test:
  timeout_ms: 5000
  parallel: auto             # auto, or number
  fail_fast: false
  categories:
    - valid
    - invalid
    - edge-case

# Benchmark settings
benchmark:
  iterations: 1000
  warmup: 100
  timeout_seconds: 60

# Cache settings
cache:
  enabled: true
  directory: ~/.ups/cache
  max_size_mb: 1024
  ttl_hours: 24

# Library preferences
libraries:
  preferred:
    csv: python:polars       # Default library for CSV
    json: dotnet:system.text.json

# CI/CD settings
ci:
  fail_on_warning: true
  output_format: junit
  output_dir: ./test-results
```

---

## 6. Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | Test failures (some tests failed) |
| 2 | Command error (invalid arguments) |
| 3 | Spec validation error |
| 4 | Adapter error (adapter crashed/unavailable) |
| 5 | Library error (library not found) |
| 10 | Internal error |

---

## 7. Environment Variables

| Variable | Description |
|----------|-------------|
| `UPS_CONFIG` | Path to config file |
| `UPS_CACHE_DIR` | Cache directory |
| `UPS_NO_COLOR` | Disable colored output |
| `UPS_ADAPTER_PATH` | Additional adapter search path |
| `UPS_LOG_LEVEL` | Log level (debug, info, warn, error) |

---

## 8. Shell Completion

```bash
# Bash
ups completion bash > /etc/bash_completion.d/ups

# Zsh
ups completion zsh > ~/.zsh/completions/_ups

# Fish
ups completion fish > ~/.config/fish/completions/ups.fish

# PowerShell
ups completion powershell >> $PROFILE
```

---

## 9. CI/CD Integration

### GitHub Actions

```yaml
# .github/workflows/parser-tests.yml
name: Parser Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install UPS Runtime
        run: pip install ups-runtime

      - name: Run Conformance Tests
        run: |
          ups test specs/csv-parser.ups.yaml \
            --all \
            --format junit \
            -o test-results/results.xml

      - name: Upload Results
        uses: actions/upload-artifact@v4
        with:
          name: test-results
          path: test-results/
```

### GitLab CI

```yaml
# .gitlab-ci.yml
parser-tests:
  image: python:3.11
  script:
    - pip install ups-runtime
    - ups test specs/csv-parser.ups.yaml --all --format junit -o results.xml
  artifacts:
    reports:
      junit: results.xml
```

---

*End of CLI Design Document*
