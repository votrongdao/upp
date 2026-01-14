# Universal Parser Specification (UPS) v1.0

## Official Specification Document

**Version:** 1.0.0
**Status:** Draft
**Last Updated:** January 2025
**Editors:** Universal Parser Platform Working Group

---

## Abstract

The Universal Parser Specification (UPS) defines a standardized, machine-readable format for describing parser specifications across all programming languages, domains, and parser paradigms. This specification enables:

1. **Interoperability**: Parser specifications can be shared, validated, and implemented across different languages and platforms
2. **Quality Measurement**: Standardized conformance testing and quality scoring
3. **Discovery**: Searchable registry of parser specifications
4. **Competition**: Fair comparison of parser implementations

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Conformance Requirements](#2-conformance-requirements)
3. [Terminology](#3-terminology)
4. [Specification Structure](#4-specification-structure)
5. [Metadata](#5-metadata)
6. [Parser Definition](#6-parser-definition)
7. [Input Specification](#7-input-specification)
8. [Output Specification](#8-output-specification)
9. [Conformance Suite](#9-conformance-suite)
10. [Quality Requirements](#10-quality-requirements)
11. [Extensions](#11-extensions)
12. [Security Considerations](#12-security-considerations)
13. [IANA Considerations](#13-iana-considerations)
14. [References](#14-references)
15. [Appendices](#15-appendices)

---

## 1. Introduction

### 1.1 Purpose

The Universal Parser Specification addresses a fundamental gap in the software ecosystem: the lack of a standardized way to describe, share, and compare parser implementations. While numerous grammar notations exist (ABNF, EBNF, PEG, etc.), there is no universal standard for:

- Describing parser behavior beyond syntax (semantics, error handling, modes)
- Defining conformance test suites
- Specifying quality requirements
- Enabling cross-language parser comparison

UPS fills this gap by providing a comprehensive, extensible schema that can describe ANY parser specification.

### 1.2 Scope

This specification covers:

- **Text parsers**: JSON, XML, YAML, CSV, configuration files, markup languages
- **Binary parsers**: Protocol Buffers, MessagePack, BSON, custom binary formats
- **Protocol parsers**: HTTP, DNS, SMTP, WebSocket, custom protocols
- **Language parsers**: Programming languages, DSLs, query languages
- **Streaming parsers**: Real-time data streams, log files, event streams
- **Hybrid parsers**: Combinations of the above

### 1.3 Goals

1. **Universality**: Describe any parser, regardless of paradigm or domain
2. **Precision**: Unambiguous specification of parser behavior
3. **Testability**: Built-in support for conformance testing
4. **Extensibility**: Allow vendor-specific extensions without breaking compatibility
5. **Simplicity**: Easy to read, write, and validate
6. **Adoption**: Low barrier to entry, high value for adoption

### 1.4 Non-Goals

- **Implementation prescription**: UPS does not mandate HOW parsers should be implemented
- **Performance guarantees**: UPS does not guarantee performance (but enables measurement)
- **Grammar generation**: UPS is not a parser generator (but can feed into one)

### 1.5 Design Philosophy

#### 1.5.1 Convention over Configuration

UPS provides sensible defaults for all optional fields, allowing simple specs to be concise while complex specs can be fully detailed.

#### 1.5.2 Progressive Disclosure

The schema is organized in layers:
- **Level 1 (Required)**: Minimum viable spec (metadata + basic input/output)
- **Level 2 (Recommended)**: Conformance tests and quality requirements
- **Level 3 (Complete)**: Full specification with all optional components

#### 1.5.3 Grammar Agnosticism

UPS supports multiple grammar notations (ABNF, EBNF, PEG, etc.) without favoring any. The grammar is treated as a black box that defines valid syntax.

#### 1.5.4 Separation of Concerns

```
┌─────────────────────────────────────────────────────────────────┐
│                        UPS Specification                         │
├─────────────────┬─────────────────┬─────────────────────────────┤
│    Metadata     │ Parser Def      │    Quality & Conformance    │
│    (WHAT)       │ (HOW)           │    (VALIDATION)             │
├─────────────────┼─────────────────┼─────────────────────────────┤
│ - Identity      │ - Input format  │ - Test vectors              │
│ - Versioning    │ - Output format │ - Property tests            │
│ - References    │ - Modes         │ - Fuzzing config            │
│ - Licensing     │ - Features      │ - Benchmarks                │
│ - Authorship    │ - Error handling│ - Security requirements     │
└─────────────────┴─────────────────┴─────────────────────────────┘
```

---

## 2. Conformance Requirements

### 2.1 Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119].

### 2.2 Specification Conformance

A UPS specification document is **conformant** if:

1. It is valid JSON or YAML
2. It validates against the UPS JSON Schema
3. It contains all REQUIRED fields
4. All referenced resources are accessible

### 2.3 Implementation Conformance Levels

| Level | Name | Requirements |
|-------|------|--------------|
| 0 | Minimal | Parse all valid inputs; reject all invalid inputs |
| 1 | Standard | Level 0 + pass all edge-case tests |
| 2 | Strict | Level 1 + pass all security tests + property tests |
| 3 | Certified | Level 2 + pass fuzzing (0 crashes) + meet performance baselines |

### 2.4 Validator Conformance

A UPS validator is conformant if it:

1. Correctly validates all valid UPS specifications
2. Correctly rejects all invalid UPS specifications
3. Provides meaningful error messages for validation failures

---

## 3. Terminology

### 3.1 Definitions

**Parser**: A software component that transforms input (text or binary) into a structured representation.

**Specification**: A formal description of a parser's expected behavior, including input format, output format, and conformance requirements.

**Grammar**: A formal description of valid input syntax, expressed in a notation such as ABNF, EBNF, or PEG.

**AST (Abstract Syntax Tree)**: A tree representation of input that captures semantic structure while discarding syntactic details.

**CST (Concrete Syntax Tree)**: A tree representation that preserves all syntactic details including whitespace and comments.

**Test Vector**: A single test case consisting of input and expected output (or error).

**Property Test**: A test that verifies an invariant property holds for generated inputs.

**Conformance Level**: A tier indicating how completely an implementation satisfies the specification.

### 3.2 Parser Categories

| Category | Description | Examples |
|----------|-------------|----------|
| Text | Character-based input | JSON, XML, CSV |
| Binary | Byte-based input | Protobuf, MessagePack |
| Structured | Hierarchical output | JSON, XML, AST |
| Streaming | Process input incrementally | SAX, Line-by-line |
| Hybrid | Combination | HTTP (text headers + binary body) |
| Incremental | Update on changes | Tree-sitter, IDE parsers |
| Declarative | Grammar-driven | ANTLR-generated |

### 3.3 Grammar Types

| Type | Full Name | Use Case |
|------|-----------|----------|
| ABNF | Augmented Backus-Naur Form | RFCs, protocols |
| EBNF | Extended BNF | ISO standards, W3C |
| PEG | Parsing Expression Grammar | Custom languages |
| ANTLR4 | ANTLR version 4 | Parser generators |
| Kaitai | Kaitai Struct | Binary formats |
| ASN.1 | Abstract Syntax Notation One | Telecom, security |
| Regex | Regular Expressions | Simple patterns |

---

## 4. Specification Structure

### 4.1 Document Format

UPS specifications MUST be expressed in JSON or YAML. YAML is RECOMMENDED for human authoring due to readability.

### 4.2 Root Structure

```yaml
ups_version: "1.0"           # REQUIRED: Schema version
metadata: { ... }            # REQUIRED: Specification metadata
parser: { ... }              # REQUIRED: Parser definition
conformance: { ... }         # OPTIONAL: Test suite
quality: { ... }             # OPTIONAL: Quality requirements
extensions: { ... }          # OPTIONAL: Custom extensions
```

### 4.3 Schema Reference

The canonical JSON Schema is available at:
```
https://universalparser.org/schema/ups/v1.0/ups-schema.json
```

### 4.4 File Naming Convention

UPS specification files SHOULD follow this naming pattern:
```
{parser-name}.ups.yaml
{parser-name}.ups.json
```

Examples:
- `json-parser.ups.yaml`
- `http-request.ups.yaml`
- `protobuf-wire.ups.yaml`

---

## 5. Metadata

### 5.1 Purpose

Metadata provides identity, versioning, and discoverability information for the specification.

### 5.2 Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | URI | Globally unique identifier |
| `name` | String | Machine-readable name (kebab-case) |
| `version` | SemVer | Semantic version |

### 5.3 Identifier Guidelines

The `id` field SHOULD follow one of these patterns:

```
# URN format (recommended for standards)
urn:ups:parser:{domain}:{name}:{version}
urn:ups:parser:ietf:json:rfc8259

# URL format (for organizational specs)
https://example.org/ups/specs/{name}/{version}
https://universalparser.org/specs/json/1.0.0
```

### 5.4 Naming Convention

The `name` field:
- MUST be lowercase
- MUST use hyphens for word separation
- MUST start with a letter
- MUST be 2-64 characters

Valid: `json-parser`, `http-request-parser`, `x509-certificate`
Invalid: `JSON_Parser`, `HTTPRequest`, `123-parser`

### 5.5 Version Semantics

Versions follow [Semantic Versioning 2.0.0]:

- **MAJOR**: Breaking changes to spec (input format, output schema)
- **MINOR**: Backward-compatible additions (new modes, tests)
- **PATCH**: Bug fixes, documentation

### 5.6 Status Lifecycle

```
draft → experimental → stable → deprecated → retired
```

| Status | Meaning |
|--------|---------|
| draft | Under development, may change significantly |
| experimental | Feature-complete but not production-ready |
| stable | Production-ready, changes follow semver |
| deprecated | Superseded, avoid new implementations |
| retired | No longer supported |

### 5.7 References

References link to normative standards. Required fields:

| Field | Required | Description |
|-------|----------|-------------|
| `type` | Yes | Reference type (rfc, iso, w3c, etc.) |
| `identifier` | Conditional | Standard identifier (e.g., "RFC 8259") |
| `url` | Conditional | URL if no standard identifier |
| `normative` | No | Whether the reference is normative (default: false) |

Example:
```yaml
references:
  - type: rfc
    identifier: "RFC 8259"
    title: "The JavaScript Object Notation (JSON) Data Interchange Format"
    url: https://www.rfc-editor.org/rfc/rfc8259
    normative: true
  - type: academic
    title: "Parsing Expression Grammars"
    url: https://bford.info/pub/lang/peg.pdf
    authors: ["Bryan Ford"]
    date: "2004-01-14"
```

### 5.8 Complete Metadata Example

```yaml
metadata:
  id: "urn:ups:parser:ietf:json:rfc8259"
  name: "json-parser"
  display_name: "JSON Parser (RFC 8259)"
  version: "1.0.0"
  status: stable
  category:
    primary: structured
    secondary: [text, context-free]
    data_flow: pull
  domain: "data-interchange"
  tags: [json, rfc8259, ecma-404, text, data, interchange]
  description: |
    A parser for JSON (JavaScript Object Notation) as defined in RFC 8259.
    JSON is a lightweight data-interchange format that is easy for humans
    to read and write and easy for machines to parse and generate.
  license:
    spdx: "CC-BY-4.0"
  authors:
    - name: "Douglas Crockford"
      organization: "JSON.org"
  references:
    - type: rfc
      identifier: "RFC 8259"
      normative: true
    - type: ecma
      identifier: "ECMA-404"
      normative: true
  created_at: "2025-01-01T00:00:00Z"
  updated_at: "2025-01-15T00:00:00Z"
```

---

## 6. Parser Definition

### 6.1 Overview

The `parser` object defines the core parsing behavior:

```yaml
parser:
  input: { ... }        # What the parser consumes
  output: { ... }       # What the parser produces
  modes: [ ... ]        # Different parsing modes
  features: { ... }     # Optional capabilities
  error_handling: { }   # Error recovery strategy
  state_machine: { }    # For protocol parsers
  dependencies: [ ]     # For composed parsers
```

### 6.2 Input vs Output Principle

UPS strictly separates input definition from output definition:

```
┌─────────────────┐                    ┌─────────────────┐
│      INPUT      │                    │     OUTPUT      │
│                 │                    │                 │
│ - Grammar       │                    │ - AST Schema    │
│ - Encoding      │   ┌──────────┐     │ - Error Format  │
│ - Streaming     │ → │  PARSER  │ →   │ - Events        │
│ - Framing       │   └──────────┘     │ - Tokens        │
│ - Constraints   │                    │                 │
└─────────────────┘                    └─────────────────┘
```

### 6.3 Modes

Modes allow a single specification to define multiple parsing behaviors:

```yaml
modes:
  - id: strict
    name: "Strict Mode"
    description: "Rejects any input that deviates from RFC 8259"
    default: true
    options:
      allow_comments: false
      allow_trailing_comma: false

  - id: lenient
    name: "Lenient Mode"
    description: "Accepts common extensions like comments and trailing commas"
    options:
      allow_comments: true
      allow_trailing_comma: true
      allow_single_quotes: true
```

### 6.4 Features Matrix

| Feature | Description | Use Case |
|---------|-------------|----------|
| `validation.schema_validation` | Validate against schema | JSON Schema validation |
| `transformation.normalization` | Normalize output | Unicode normalization |
| `transformation.canonicalization` | Canonical form | Digital signatures |
| `querying.jsonpath` | JSONPath support | Data extraction |
| `modification.mutable_ast` | Modifiable AST | Editors, transformers |
| `modification.serialization` | Serialize back | Round-trip support |

---

## 7. Input Specification

### 7.1 Grammar Definition

The `format` object defines the input grammar:

```yaml
input:
  format:
    type: abnf                    # Grammar notation
    variant: rfc5234              # Specific variant
    grammar: |                    # Inline grammar
      JSON-text = ws value ws
      value = false / null / true / object / array / number / string
      ...
    entry_rule: JSON-text         # Start rule
```

#### 7.1.1 Supported Grammar Types

| Type | Description | Best For |
|------|-------------|----------|
| `abnf` | RFC 5234 ABNF | IETF standards, RFCs |
| `ebnf` | Extended BNF | ISO standards |
| `peg` | Parsing Expression Grammar | Unambiguous parsing |
| `antlr4` | ANTLR 4 grammar | Complex languages |
| `regex` | Regular expression | Simple patterns |
| `tree-sitter` | Tree-sitter grammar | IDE integration |
| `kaitai` | Kaitai Struct | Binary formats |
| `asn1` | ASN.1 | Telecom, certificates |
| `protobuf-schema` | Protocol Buffers | gRPC, serialization |
| `json-schema` | JSON Schema | JSON validation |
| `custom` | Custom notation | Proprietary formats |
| `prose` | Natural language | Complex semantics |
| `reference` | External reference | Reuse existing |

#### 7.1.2 External Grammar References

For large grammars, use external files:

```yaml
format:
  type: antlr4
  grammar_file: "./grammars/JSON.g4"
  entry_rule: json
```

Or multiple files:

```yaml
format:
  type: antlr4
  grammar_files:
    - "./grammars/JSONLexer.g4"
    - "./grammars/JSONParser.g4"
  entry_rule: json
```

### 7.2 Binary Structure Definition

For binary formats, use `binary_structure`:

```yaml
format:
  type: custom
  binary_structure:
    endianness: big
    alignment: 4
    magic_bytes:
      offset: 0
      bytes: "89504E47"    # PNG magic
    fields:
      - name: magic
        type: bytes
        size: 8
      - name: chunk_length
        type: uint32
      - name: chunk_type
        type: string
        size: 4
        encoding: ascii
      - name: chunk_data
        type: bytes
        size: chunk_length
      - name: crc
        type: uint32
```

### 7.3 Encoding Specification

```yaml
encoding:
  default: utf-8
  supported: [utf-8, utf-16le, utf-16be, utf-32]
  bom: optional          # required | optional | forbidden | detect
  newline: any           # lf | crlf | cr | any | preserve
  null_handling: escape  # allowed | forbidden | escape
```

### 7.4 Streaming Specification

```yaml
streaming:
  supported: true
  mode: chunked          # chunked | incremental | sax-style | pull | push
  chunk_boundaries: any  # any | line | record | message | frame
  minimum_chunk_size: 1
  buffering:
    required: true
    max_buffer_size: 65536
    lookahead: 4
  backtracking: false
  resumable: true
```

### 7.5 Framing Specification

For protocols with message framing:

```yaml
framing:
  type: length-prefixed
  length_field:
    offset: 0
    size: 4
    endianness: big
    includes_header: false
```

Or delimiter-based:

```yaml
framing:
  type: delimiter
  delimiter:
    end: "\r\n\r\n"
    escape: null
```

### 7.6 Input Constraints

```yaml
constraints:
  max_size: 10485760        # 10 MB
  max_depth: 100
  max_width: 10000
  max_string_length: 1048576
  max_number_magnitude: 1e308
  max_array_length: 100000
  max_object_keys: 10000
  custom:
    max_unicode_codepoint: 1114111
```

---

## 8. Output Specification

### 8.1 Output Formats

UPS supports multiple output formats:

| Format | Description | Use Case |
|--------|-------------|----------|
| `ast` | Abstract Syntax Tree | Semantic processing |
| `cst` | Concrete Syntax Tree | Formatters, linters |
| `dom` | Document Object Model | XML, HTML |
| `object` | Native language object | Simple deserialization |
| `events` | Event stream | SAX-style processing |
| `tokens` | Token stream | Lexical analysis |
| `bytes` | Binary output | Transcoding |

### 8.2 AST Schema Definition

```yaml
output:
  primary:
    type: ast
    schema_type: json-schema
    schema:
      $schema: "https://json-schema.org/draft/2020-12/schema"
      title: "JSON AST"
      oneOf:
        - $ref: "#/$defs/object"
        - $ref: "#/$defs/array"
        - $ref: "#/$defs/string"
        - $ref: "#/$defs/number"
        - $ref: "#/$defs/boolean"
        - $ref: "#/$defs/null"
      $defs:
        object:
          type: object
          properties:
            type: { const: "object" }
            members:
              type: array
              items:
                type: object
                properties:
                  key: { type: string }
                  value: { $ref: "#" }
        # ... other definitions
    preserves: [positions]
```

### 8.3 Node Type Definitions

For complex parsers, define node types explicitly:

```yaml
output:
  primary:
    type: ast
    node_types:
      - name: Program
        kind: node
        fields:
          statements:
            type: Statement
            multiple: true

      - name: IfStatement
        kind: node
        fields:
          condition:
            type: Expression
          then_branch:
            type: Statement
          else_branch:
            type: Statement
            optional: true

      - name: Identifier
        kind: token
        fields:
          name:
            type: string
```

### 8.4 Error Output Specification

```yaml
output:
  errors:
    schema:
      type: object
      required: [code, message, position]
      properties:
        code:
          type: string
        message:
          type: string
        severity:
          type: string
          enum: [error, warning, info, hint]
        position:
          type: object
          properties:
            offset: { type: integer }
            line: { type: integer }
            column: { type: integer }
        context:
          type: string
        suggestions:
          type: array
          items:
            type: string

    codes:
      - code: E001
        message: "Unexpected token"
        severity: error
        category: syntax
        recoverable: true

      - code: E002
        message: "Unterminated string"
        severity: error
        category: syntax
        recoverable: false

      - code: W001
        message: "Trailing comma"
        severity: warning
        category: style
        recoverable: true

    includes_position: true
    includes_context: true
    includes_suggestions: true
```

### 8.5 Event Stream Output

For SAX-style parsers:

```yaml
output:
  events:
    events:
      - name: start_document
        data: {}

      - name: end_document
        data: {}

      - name: start_object
        data:
          type: object
          properties:
            depth: { type: integer }

      - name: end_object
        data: {}

      - name: key
        data:
          type: object
          properties:
            value: { type: string }

      - name: value
        data:
          type: object
          properties:
            type: { type: string }
            value: {}

    ordering: depth-first
```

---

## 9. Conformance Suite

### 9.1 Purpose

The conformance suite provides a standardized way to verify that parser implementations correctly follow the specification.

### 9.2 Test Vector Structure

```yaml
conformance:
  version: "1.0.0"

  test_vectors:
    - id: valid-001
      name: "Empty object"
      description: "Parser must accept empty object"
      category: valid
      priority: critical
      input:
        inline: "{}"
      expected:
        success:
          ast:
            type: object
            members: []

    - id: invalid-001
      name: "Trailing comma in object"
      category: invalid
      input:
        inline: '{"a": 1,}'
      expected:
        error:
          code: E001
          message_pattern: ".*trailing comma.*"
```

### 9.3 Test Categories

| Category | Description | Required for Level |
|----------|-------------|-------------------|
| `valid` | Must parse successfully | 0+ |
| `invalid` | Must reject with error | 0+ |
| `edge-case` | Edge cases that must be handled | 1+ |
| `security` | Security-related tests | 2+ |
| `performance` | Performance benchmarks | 3 |
| `regression` | Bug fix verification | - |

### 9.4 Input Sources

Tests can provide input in multiple ways:

```yaml
# Inline (for short inputs)
input:
  inline: '{"key": "value"}'

# Base64 (for binary or special characters)
input:
  inline_base64: "eyJrZXkiOiAidmFsdWUifQ=="

# Hex (for binary)
input:
  inline_hex: "7b226b6579223a202276616c7565227d"

# External file
input:
  file: "./test-data/large-file.json"

# URL
input:
  url: "https://example.org/test-data/sample.json"

# Generator (for property tests)
input:
  generator:
    type: random
    params:
      min_length: 100
      max_length: 10000
```

### 9.5 Expected Outcomes

```yaml
# Success with specific AST
expected:
  success:
    ast: { type: object, members: [] }

# Success with properties (for generated input)
expected:
  success:
    properties:
      is_valid: true
      node_count: { min: 1 }

# Error with specific code
expected:
  error:
    code: E001

# Error matching pattern
expected:
  error:
    message_pattern: "unexpected.*at position"

# Multiple valid outcomes
expected:
  either:
    - success: { ast: { ... } }
    - error: { code: E001 }

# Implementation-defined
expected:
  implementation_defined: true
```

### 9.6 Importing External Test Suites

```yaml
conformance:
  imports:
    - url: "https://github.com/nst/JSONTestSuite"
      type: json-test-suite
      version: "main"
      filter:
        include: ["y_*", "n_*"]
        exclude: ["i_*"]  # Implementation-defined

    - url: "https://github.com/json-schema-org/JSON-Schema-Test-Suite"
      type: json-schema-test-suite
      filter:
        include: ["draft2020-12/**"]
```

### 9.7 Property Tests

```yaml
conformance:
  property_tests:
    - id: roundtrip
      name: "Parse-serialize roundtrip"
      property: roundtrip
      description: |
        For any valid JSON, parse(input) then serialize(ast) should
        produce semantically equivalent output.
      iterations: 100000
      seed: 42
      shrinking: true

    - id: deterministic
      name: "Deterministic parsing"
      property: deterministic
      description: |
        Parsing the same input twice should produce identical AST.
      iterations: 10000

    - id: no-crash
      name: "No crash on any input"
      property: no-crash
      iterations: 1000000
      generator:
        type: random
        params:
          include_invalid: true
```

### 9.8 Fuzzing Configuration

```yaml
conformance:
  fuzzing:
    enabled: true
    min_iterations: 1000000
    timeout_seconds: 3600

    corpus:
      - "./fuzz-corpus/"
      - "https://github.com/dvyukov/go-fuzz-corpus/tree/master/json"

    dictionary:
      - "true"
      - "false"
      - "null"
      - '\"'
      - '\\'
      - "\\u0000"

    strategies:
      - mutation
      - grammar-based
      - coverage-guided

    targets:
      - crash
      - hang
      - memory
      - undefined-behavior
```

---

## 10. Quality Requirements

### 10.1 Overview

Quality requirements define minimum standards for implementations:

```yaml
quality:
  conformance: { ... }
  performance: { ... }
  security: { ... }
  maintainability: { ... }
```

### 10.2 Conformance Requirements

```yaml
quality:
  conformance:
    minimum_level: level_1
    required_tests:
      - valid-001
      - valid-002
      - invalid-001
    required_categories:
      - valid
      - invalid
      - edge-case
```

### 10.3 Performance Requirements

```yaml
quality:
  performance:
    throughput:
      minimum_mbps: 100      # Minimum acceptable
      target_mbps: 500       # Target for high quality

    latency:
      p50_max_us: 100        # 50th percentile
      p99_max_us: 1000       # 99th percentile
      p999_max_us: 10000     # 99.9th percentile

    memory:
      max_multiplier: 3      # Max memory = 3x input size
      max_absolute_mb: 1024  # Absolute max

    startup:
      max_cold_start_ms: 100
      max_warm_start_ms: 10
```

### 10.4 Security Requirements

```yaml
quality:
  security:
    vulnerability_tolerance:
      critical: 0     # Zero critical vulnerabilities
      high: 0         # Zero high vulnerabilities
      medium: 2       # Up to 2 medium
      low: 5          # Up to 5 low

    required_protections:
      - stack-overflow-protection    # Max recursion depth
      - dos-protection               # Resource limits
      - billion-laughs-protection    # Entity expansion limits
      - xxe-protection               # External entity handling

    forbidden_patterns:
      - "eval("
      - "exec("
      - "system("
```

### 10.5 Maintainability Requirements

```yaml
quality:
  maintainability:
    documentation: full        # none | basic | full
    test_coverage_percent: 80
    max_complexity: 20         # Cyclomatic complexity
```

---

## 11. Extensions

### 11.1 Extension Mechanism

UPS supports custom extensions via the `extensions` field:

```yaml
extensions:
  x-mycompany:
    internal_id: "PARSER-001"
    approval_status: "approved"

  x-benchmark:
    custom_metrics:
      - name: "allocations_per_kb"
        unit: "count"
```

### 11.2 Extension Namespaces

- **`x-*`**: Vendor-specific extensions
- **`ups-exp-*`**: Experimental UPS features
- **`ups-draft-*`**: Draft UPS features

### 11.3 Extension Guidelines

1. Extensions MUST NOT conflict with core schema
2. Extensions SHOULD be documented
3. Extensions MUST NOT be required for basic functionality
4. Validators MUST ignore unknown extensions

---

## 12. Security Considerations

### 12.1 Specification Security

UPS specifications may contain:
- Executable grammar rules
- External resource references
- Test vectors with malicious payloads

Validators MUST:
- Sandbox grammar execution
- Validate external URLs before fetching
- Limit resource consumption

### 12.2 Implementation Security

Implementations MUST protect against:

| Vulnerability | Mitigation |
|---------------|------------|
| Stack overflow | Depth limits |
| Memory exhaustion | Size limits |
| CPU exhaustion | Timeout limits |
| Billion laughs | Entity limits |
| XXE attacks | Disable external entities |
| ReDoS | Regex complexity limits |

### 12.3 Test Vector Security

Test vectors with category `security` are specifically designed to test vulnerabilities. These tests:
- MUST NOT be executed with elevated privileges
- SHOULD be executed in isolated environments
- MUST have clear documentation of risks

---

## 13. IANA Considerations

### 13.1 Media Type Registration

```
Type name: application
Subtype name: ups+json
Required parameters: N/A
Optional parameters: version
Encoding: UTF-8
```

### 13.2 File Extension

```
Extension: .ups.json, .ups.yaml
```

---

## 14. References

### 14.1 Normative References

- [RFC 2119] Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels"
- [RFC 8259] Bray, T., Ed., "The JavaScript Object Notation (JSON) Data Interchange Format"
- [RFC 5234] Crocker, D., Ed., "Augmented BNF for Syntax Specifications: ABNF"
- [JSON Schema] Wright, A., et al., "JSON Schema"
- [Semantic Versioning 2.0.0] Preston-Werner, T.

### 14.2 Informative References

- [ANTLR4] Parr, T., "The Definitive ANTLR 4 Reference"
- [Tree-sitter] Brunsfeld, M., "Tree-sitter"
- [Kaitai Struct] https://kaitai.io/
- [PEG] Ford, B., "Parsing Expression Grammars"

---

## 15. Appendices

### Appendix A: Full JSON Schema

See `ups-schema-v1.0.json` for the complete JSON Schema.

### Appendix B: Example Specifications

See companion document: `UPS-EXAMPLES.md`

### Appendix C: Grammar Type Reference

See companion document: `UPS-GRAMMAR-REFERENCE.md`

### Appendix D: Security Test Guidelines

See companion document: `UPS-SECURITY-TESTING.md`

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2025-01-15 | Initial release |

---

## Authors

Universal Parser Platform Working Group

## License

This specification is released under CC-BY-4.0.

---

*End of Universal Parser Specification v1.0*
