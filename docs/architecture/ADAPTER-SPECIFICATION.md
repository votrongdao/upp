# Universal Adapter Interface (UAI) Specification

## Language Adapter Technical Specification

**Version:** 1.0.0
**Status:** Draft
**Last Updated:** January 2025

---

## 1. Overview

This document defines the **Universal Adapter Interface (UAI)** - the contract that all language adapters MUST implement to integrate with the UPS Runtime.

### Design Goals

1. **Language Agnostic**: Any language can implement an adapter
2. **Process Isolated**: Adapters run as separate processes for safety
3. **Stateless Operations**: Each operation is independent
4. **Streaming Support**: Handle large inputs efficiently
5. **Metrics Built-in**: Performance measurement is native

---

## 2. Communication Protocol

### 2.1 Transport Layer

Adapters communicate with UPS Runtime via **JSON-RPC 2.0** over **stdin/stdout**.

```
┌─────────────────┐                    ┌─────────────────┐
│   UPS Runtime   │                    │    Adapter      │
│                 │   stdin            │    Process      │
│                 │ ──────────────────▶│                 │
│                 │                    │                 │
│                 │   stdout           │                 │
│                 │ ◀──────────────────│                 │
│                 │                    │                 │
│                 │   stderr           │                 │
│                 │ ◀──────────────────│ (logs only)     │
└─────────────────┘                    └─────────────────┘
```

### 2.2 Message Format

#### Request (Runtime → Adapter)

```json
{
  "jsonrpc": "2.0",
  "id": "<unique-request-id>",
  "method": "<method-name>",
  "params": { ... }
}
```

#### Success Response (Adapter → Runtime)

```json
{
  "jsonrpc": "2.0",
  "id": "<matching-request-id>",
  "result": { ... }
}
```

#### Error Response (Adapter → Runtime)

```json
{
  "jsonrpc": "2.0",
  "id": "<matching-request-id>",
  "error": {
    "code": -32000,
    "message": "Error description",
    "data": { ... }
  }
}
```

### 2.3 Error Codes

| Code | Name | Description |
|------|------|-------------|
| -32700 | Parse Error | Invalid JSON |
| -32600 | Invalid Request | Not a valid JSON-RPC request |
| -32601 | Method Not Found | Method does not exist |
| -32602 | Invalid Params | Invalid method parameters |
| -32603 | Internal Error | Internal adapter error |
| -32000 | Library Error | Error from underlying library |
| -32001 | Timeout | Operation timed out |
| -32002 | Memory Limit | Memory limit exceeded |
| -32003 | Not Supported | Operation not supported |

---

## 3. Adapter Lifecycle

```
┌─────────────────────────────────────────────────────────────────┐
│                     ADAPTER LIFECYCLE                           │
└─────────────────────────────────────────────────────────────────┘

    START
      │
      ▼
┌───────────────┐
│   Spawned     │  ← Runtime spawns adapter process
│               │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│   ping()      │  ← Health check
│               │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│  get_info()   │  ← Discover capabilities
│               │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│ initialize()  │  ← Load specific library
│               │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│  [Operations] │  ← parse(), validate(), etc.
│   (repeat)    │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│  shutdown()   │  ← Graceful shutdown
│               │
└───────┬───────┘
        │
        ▼
      END
```

---

## 4. Interface Methods

### 4.1 ping

Health check to verify adapter is responsive.

**Request:**
```json
{
  "jsonrpc": "2.0",
  "id": "1",
  "method": "ping",
  "params": {}
}
```

**Response:**
```json
{
  "jsonrpc": "2.0",
  "id": "1",
  "result": {
    "alive": true,
    "uptime_ms": 12345,
    "memory_mb": 45.2
  }
}
```

### 4.2 get_info

Retrieve adapter metadata and capabilities.

**Request:**
```json
{
  "jsonrpc": "2.0",
  "id": "2",
  "method": "get_info",
  "params": {}
}
```

**Response:**
```json
{
  "jsonrpc": "2.0",
  "id": "2",
  "result": {
    "adapter": {
      "name": "ups-python-adapter",
      "version": "1.0.0",
      "protocol_version": "1.0"
    },
    "runtime": {
      "language": "python",
      "version": "3.11.5",
      "platform": "linux-x86_64"
    },
    "supported_libraries": [
      {
        "id": "python:csv-stdlib",
        "name": "csv (stdlib)",
        "version": "builtin",
        "formats": ["csv"],
        "capabilities": ["parse", "serialize"]
      },
      {
        "id": "python:pandas",
        "name": "pandas",
        "version": "2.1.0",
        "formats": ["csv", "json", "excel", "parquet"],
        "capabilities": ["parse", "serialize", "transform"]
      }
    ],
    "capabilities": [
      "streaming",
      "memory_tracking",
      "cpu_tracking"
    ]
  }
}
```

### 4.3 initialize

Initialize a specific library for use.

**Request:**
```json
{
  "jsonrpc": "2.0",
  "id": "3",
  "method": "initialize",
  "params": {
    "library_id": "python:pandas",
    "options": {
      "low_memory": true
    }
  }
}
```

**Response:**
```json
{
  "jsonrpc": "2.0",
  "id": "3",
  "result": {
    "success": true,
    "library": {
      "id": "python:pandas",
      "name": "pandas",
      "version": "2.1.0"
    },
    "active_options": {
      "low_memory": true
    }
  }
}
```

### 4.4 parse

Parse input data and return structured output.

**Request:**
```json
{
  "jsonrpc": "2.0",
  "id": "4",
  "method": "parse",
  "params": {
    "input": {
      "type": "inline",
      "data": "name,age,city\nAlice,30,NYC\nBob,25,LA",
      "encoding": "utf-8"
    },
    "format": "csv",
    "mode": "auto",
    "options": {
      "has_header": true,
      "delimiter": ","
    },
    "collect_metrics": true
  }
}
```

**Response:**
```json
{
  "jsonrpc": "2.0",
  "id": "4",
  "result": {
    "success": true,
    "output": {
      "headers": ["name", "age", "city"],
      "hasHeaders": true,
      "records": [
        ["Alice", "30", "NYC"],
        ["Bob", "25", "LA"]
      ],
      "rowCount": 2,
      "columnCount": 3
    },
    "errors": [],
    "warnings": [],
    "metrics": {
      "duration_ns": 1250000,
      "memory_bytes": 2048,
      "peak_memory_bytes": 4096,
      "cpu_time_ns": 1100000
    }
  }
}
```

### 4.5 parse (with errors)

**Response with parse errors:**
```json
{
  "jsonrpc": "2.0",
  "id": "5",
  "result": {
    "success": false,
    "output": null,
    "errors": [
      {
        "code": "CSV001",
        "message": "Unclosed quoted field",
        "severity": "error",
        "position": {
          "line": 3,
          "column": 15,
          "offset": 45
        },
        "context": "\"unclosed string"
      }
    ],
    "warnings": [],
    "metrics": {
      "duration_ns": 500000,
      "memory_bytes": 1024
    }
  }
}
```

### 4.6 parse_file

Parse from file path (for large files).

**Request:**
```json
{
  "jsonrpc": "2.0",
  "id": "6",
  "method": "parse_file",
  "params": {
    "path": "/tmp/ups-test/large-data.csv",
    "format": "csv",
    "mode": "strict",
    "options": {
      "has_header": true
    },
    "collect_metrics": true
  }
}
```

### 4.7 validate

Validate input without full parsing.

**Request:**
```json
{
  "jsonrpc": "2.0",
  "id": "7",
  "method": "validate",
  "params": {
    "input": {
      "type": "inline",
      "data": "{\"name\": \"test\"}"
    },
    "format": "json",
    "schema": {
      "type": "object",
      "required": ["name"],
      "properties": {
        "name": { "type": "string" }
      }
    }
  }
}
```

**Response:**
```json
{
  "jsonrpc": "2.0",
  "id": "7",
  "result": {
    "valid": true,
    "errors": [],
    "metrics": {
      "duration_ns": 50000
    }
  }
}
```

### 4.8 serialize

Convert structured data back to format.

**Request:**
```json
{
  "jsonrpc": "2.0",
  "id": "8",
  "method": "serialize",
  "params": {
    "data": {
      "headers": ["name", "age"],
      "records": [["Alice", "30"]]
    },
    "format": "csv",
    "options": {
      "include_header": true,
      "delimiter": ","
    }
  }
}
```

**Response:**
```json
{
  "jsonrpc": "2.0",
  "id": "8",
  "result": {
    "success": true,
    "output": "name,age\nAlice,30\n",
    "encoding": "utf-8",
    "metrics": {
      "duration_ns": 100000
    }
  }
}
```

### 4.9 get_options

Get available options for a library/format combination.

**Request:**
```json
{
  "jsonrpc": "2.0",
  "id": "9",
  "method": "get_options",
  "params": {
    "library_id": "python:pandas",
    "format": "csv"
  }
}
```

**Response:**
```json
{
  "jsonrpc": "2.0",
  "id": "9",
  "result": {
    "options": [
      {
        "name": "delimiter",
        "type": "string",
        "default": ",",
        "description": "Field delimiter",
        "ups_mapping": "parser.modes.*.options.delimiter"
      },
      {
        "name": "has_header",
        "type": "boolean",
        "default": true,
        "description": "First row is header",
        "ups_mapping": "parser.modes.*.options.has_header"
      },
      {
        "name": "encoding",
        "type": "string",
        "default": "utf-8",
        "description": "File encoding",
        "ups_mapping": "parser.input.encoding.default"
      },
      {
        "name": "low_memory",
        "type": "boolean",
        "default": false,
        "description": "Use chunked reading for large files",
        "ups_mapping": null,
        "library_specific": true
      }
    ]
  }
}
```

### 4.10 shutdown

Graceful shutdown.

**Request:**
```json
{
  "jsonrpc": "2.0",
  "id": "99",
  "method": "shutdown",
  "params": {}
}
```

**Response:**
```json
{
  "jsonrpc": "2.0",
  "id": "99",
  "result": {
    "success": true
  }
}
```

---

## 5. Input Specification

### 5.1 Input Types

```yaml
input_types:
  inline:
    description: "Data provided directly in request"
    fields:
      type: "inline"
      data: string          # Raw data
      encoding: string      # "utf-8", "base64", "hex"

  inline_base64:
    description: "Base64 encoded binary data"
    fields:
      type: "inline_base64"
      data: string          # Base64 encoded

  file:
    description: "Read from file path"
    fields:
      type: "file"
      path: string          # Absolute file path

  stream:
    description: "Streaming input (multiple chunks)"
    fields:
      type: "stream"
      stream_id: string     # ID for subsequent chunks
```

### 5.2 Large Input Handling

For inputs > 1MB, use file-based input:

```json
{
  "method": "parse",
  "params": {
    "input": {
      "type": "file",
      "path": "/tmp/ups-runtime/inputs/large-file.csv"
    }
  }
}
```

### 5.3 Streaming Protocol

For very large inputs, use streaming:

```json
// Start stream
{
  "method": "stream_start",
  "params": {
    "stream_id": "stream-123",
    "format": "csv",
    "total_size": 104857600,
    "options": {}
  }
}

// Send chunks
{
  "method": "stream_chunk",
  "params": {
    "stream_id": "stream-123",
    "chunk_index": 0,
    "data": "base64-encoded-chunk",
    "is_last": false
  }
}

// Final chunk
{
  "method": "stream_chunk",
  "params": {
    "stream_id": "stream-123",
    "chunk_index": 10,
    "data": "base64-encoded-chunk",
    "is_last": true
  }
}

// Get result
{
  "method": "stream_result",
  "params": {
    "stream_id": "stream-123"
  }
}
```

---

## 6. Output Specification

### 6.1 Output Normalization

All adapters MUST normalize output to match UPS spec output schema.

```yaml
# UPS CSV Output Schema (from csv-parser.ups.yaml)
output:
  headers: string[]      # Column headers if present
  hasHeaders: boolean    # Whether headers were detected
  records: string[][]    # Row data as arrays
  rowCount: integer      # Number of data rows
  columnCount: integer   # Number of columns

# Adapter MUST transform library output to this schema
# Example: pandas DataFrame → UPS CSV output

pandas_output:
  columns: ["name", "age"]
  data: [["Alice", 30], ["Bob", 25]]

normalized_output:
  headers: ["name", "age"]
  hasHeaders: true
  records: [["Alice", "30"], ["Bob", "25"]]  # Note: string conversion
  rowCount: 2
  columnCount: 2
```

### 6.2 Error Normalization

Map library-specific errors to UPS error codes:

```yaml
error_mapping:
  # pandas errors → UPS CSV errors
  "ParserError: Error tokenizing data":
    ups_code: "CSV001"
    ups_message: "Unclosed quoted field"

  "EmptyDataError":
    ups_code: "CSV004"
    ups_message: "Empty file"

  "UnicodeDecodeError":
    ups_code: "CSV005"
    ups_message: "Invalid UTF-8 sequence"
```

---

## 7. Metrics Collection

### 7.1 Required Metrics

Every parse operation MUST collect:

```yaml
required_metrics:
  duration_ns:
    description: "Wall-clock time in nanoseconds"
    type: integer

  memory_bytes:
    description: "Memory used during operation"
    type: integer
```

### 7.2 Optional Metrics

```yaml
optional_metrics:
  peak_memory_bytes:
    description: "Peak memory during operation"
    type: integer

  cpu_time_ns:
    description: "CPU time consumed"
    type: integer

  gc_collections:
    description: "Garbage collection events"
    type: integer
    language_specific: [python, dotnet, java]

  allocations:
    description: "Number of memory allocations"
    type: integer
    language_specific: [dotnet, rust]

  input_size_bytes:
    description: "Size of input processed"
    type: integer

  output_size_bytes:
    description: "Size of output produced"
    type: integer
```

### 7.3 Metrics Collection Implementation

```python
# Python example
import time
import tracemalloc

def collect_metrics(func):
    def wrapper(*args, **kwargs):
        tracemalloc.start()
        start_time = time.perf_counter_ns()

        result = func(*args, **kwargs)

        duration_ns = time.perf_counter_ns() - start_time
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
    return wrapper
```

---

## 8. Library Binding Implementation

### 8.1 Binding Interface

```python
# Python binding interface
from abc import ABC, abstractmethod
from typing import Any, Dict, List, Optional

class LibraryBinding(ABC):
    """Base class for library bindings."""

    @property
    @abstractmethod
    def library_id(self) -> str:
        """Unique identifier: 'python:pandas'"""
        pass

    @property
    @abstractmethod
    def library_name(self) -> str:
        """Display name: 'pandas'"""
        pass

    @property
    @abstractmethod
    def library_version(self) -> str:
        """Library version"""
        pass

    @property
    @abstractmethod
    def supported_formats(self) -> List[str]:
        """Supported format IDs: ['csv', 'json']"""
        pass

    @property
    @abstractmethod
    def capabilities(self) -> List[str]:
        """Capabilities: ['parse', 'serialize', 'validate']"""
        pass

    @abstractmethod
    def initialize(self, options: Dict[str, Any]) -> None:
        """Initialize the library with options."""
        pass

    @abstractmethod
    def parse(
        self,
        data: bytes,
        format_id: str,
        options: Dict[str, Any]
    ) -> Dict[str, Any]:
        """
        Parse data and return normalized UPS output.

        Args:
            data: Raw input bytes
            format_id: Format identifier ('csv', 'json')
            options: Parser options from UPS mode

        Returns:
            Normalized output matching UPS output schema
        """
        pass

    @abstractmethod
    def serialize(
        self,
        data: Dict[str, Any],
        format_id: str,
        options: Dict[str, Any]
    ) -> bytes:
        """Serialize structured data to format."""
        pass

    def validate(
        self,
        data: bytes,
        format_id: str,
        schema: Optional[Dict[str, Any]] = None
    ) -> Dict[str, Any]:
        """Validate data (optional, default uses parse)."""
        try:
            self.parse(data, format_id, {})
            return {"valid": True, "errors": []}
        except Exception as e:
            return {"valid": False, "errors": [str(e)]}

    @abstractmethod
    def get_options(self, format_id: str) -> List[Dict[str, Any]]:
        """Get available options for format."""
        pass
```

### 8.2 Example: pandas CSV Binding

```python
# bindings/pandas_csv.py
import pandas as pd
from io import BytesIO
from typing import Any, Dict, List

class PandasCsvBinding(LibraryBinding):

    @property
    def library_id(self) -> str:
        return "python:pandas"

    @property
    def library_name(self) -> str:
        return "pandas"

    @property
    def library_version(self) -> str:
        return pd.__version__

    @property
    def supported_formats(self) -> List[str]:
        return ["csv"]

    @property
    def capabilities(self) -> List[str]:
        return ["parse", "serialize", "transform"]

    def initialize(self, options: Dict[str, Any]) -> None:
        self._options = options

    def parse(
        self,
        data: bytes,
        format_id: str,
        options: Dict[str, Any]
    ) -> Dict[str, Any]:
        """Parse CSV using pandas."""

        # Map UPS options to pandas options
        pandas_options = {
            "delimiter": options.get("delimiter", ","),
            "header": 0 if options.get("has_header", True) else None,
            "encoding": options.get("encoding", "utf-8"),
            "skip_blank_lines": options.get("skip_empty_rows", True),
        }

        # Parse with pandas
        df = pd.read_csv(BytesIO(data), **pandas_options)

        # Normalize to UPS output schema
        headers = list(df.columns) if pandas_options["header"] == 0 else None
        records = df.astype(str).values.tolist()

        return {
            "headers": headers,
            "hasHeaders": headers is not None,
            "records": records,
            "rowCount": len(records),
            "columnCount": len(df.columns)
        }

    def serialize(
        self,
        data: Dict[str, Any],
        format_id: str,
        options: Dict[str, Any]
    ) -> bytes:
        """Serialize to CSV using pandas."""

        df = pd.DataFrame(
            data["records"],
            columns=data.get("headers")
        )

        output = BytesIO()
        df.to_csv(
            output,
            index=False,
            header=data.get("hasHeaders", True)
        )
        return output.getvalue()

    def get_options(self, format_id: str) -> List[Dict[str, Any]]:
        return [
            {
                "name": "delimiter",
                "type": "string",
                "default": ",",
                "description": "Field delimiter"
            },
            {
                "name": "has_header",
                "type": "boolean",
                "default": True,
                "description": "First row is header"
            },
            {
                "name": "encoding",
                "type": "string",
                "default": "utf-8",
                "description": "File encoding"
            },
            {
                "name": "skip_empty_rows",
                "type": "boolean",
                "default": True,
                "description": "Skip blank lines"
            }
        ]
```

### 8.3 Example: CsvHelper Binding (.NET)

```csharp
// Bindings/CsvHelperBinding.cs
using CsvHelper;
using CsvHelper.Configuration;
using System.Globalization;

public class CsvHelperBinding : ILibraryBinding
{
    public string LibraryId => "dotnet:csvhelper";
    public string LibraryName => "CsvHelper";
    public string LibraryVersion => typeof(CsvReader).Assembly
        .GetName().Version?.ToString() ?? "unknown";
    public string[] SupportedFormats => new[] { "csv" };
    public string[] Capabilities => new[] { "parse", "serialize", "validate" };

    public Dictionary<string, object> Parse(
        byte[] data,
        string formatId,
        Dictionary<string, object> options)
    {
        var config = new CsvConfiguration(CultureInfo.InvariantCulture)
        {
            Delimiter = options.GetValueOrDefault("delimiter", ",").ToString(),
            HasHeaderRecord = (bool)options.GetValueOrDefault("has_header", true),
        };

        using var stream = new MemoryStream(data);
        using var reader = new StreamReader(stream);
        using var csv = new CsvReader(reader, config);

        var records = new List<string[]>();
        string[]? headers = null;

        csv.Read();
        if (config.HasHeaderRecord)
        {
            csv.ReadHeader();
            headers = csv.HeaderRecord;
        }

        while (csv.Read())
        {
            var row = new string[csv.ColumnCount];
            for (int i = 0; i < csv.ColumnCount; i++)
            {
                row[i] = csv.GetField(i) ?? "";
            }
            records.Add(row);
        }

        return new Dictionary<string, object>
        {
            ["headers"] = headers,
            ["hasHeaders"] = headers != null,
            ["records"] = records,
            ["rowCount"] = records.Count,
            ["columnCount"] = headers?.Length ?? (records.FirstOrDefault()?.Length ?? 0)
        };
    }
}
```

---

## 9. Adapter Entry Point

### 9.1 Python Adapter Main

```python
# ups_adapter/main.py
import sys
import json
from typing import Dict, Any

from .interface import AdapterInterface

class JsonRpcServer:
    def __init__(self, adapter: AdapterInterface):
        self.adapter = adapter

    def run(self):
        """Main loop reading from stdin, writing to stdout."""
        for line in sys.stdin:
            try:
                request = json.loads(line.strip())
                response = self.handle_request(request)
            except json.JSONDecodeError as e:
                response = self.error_response(
                    None, -32700, f"Parse error: {e}"
                )
            except Exception as e:
                response = self.error_response(
                    request.get("id"), -32603, f"Internal error: {e}"
                )

            sys.stdout.write(json.dumps(response) + "\n")
            sys.stdout.flush()

    def handle_request(self, request: Dict[str, Any]) -> Dict[str, Any]:
        method = request.get("method")
        params = request.get("params", {})
        request_id = request.get("id")

        handler = getattr(self.adapter, method, None)
        if handler is None:
            return self.error_response(
                request_id, -32601, f"Method not found: {method}"
            )

        try:
            result = handler(**params)
            return {
                "jsonrpc": "2.0",
                "id": request_id,
                "result": result
            }
        except TypeError as e:
            return self.error_response(
                request_id, -32602, f"Invalid params: {e}"
            )
        except Exception as e:
            return self.error_response(
                request_id, -32000, f"Library error: {e}"
            )

    def error_response(
        self, request_id, code: int, message: str
    ) -> Dict[str, Any]:
        return {
            "jsonrpc": "2.0",
            "id": request_id,
            "error": {
                "code": code,
                "message": message
            }
        }


def main():
    adapter = AdapterInterface()
    server = JsonRpcServer(adapter)
    server.run()


if __name__ == "__main__":
    main()
```

### 9.2 Adapter Interface Implementation

```python
# ups_adapter/interface.py
import time
import tracemalloc
from typing import Any, Dict, List, Optional

from .bindings import AVAILABLE_BINDINGS

class AdapterInterface:
    def __init__(self):
        self._start_time = time.time()
        self._current_binding = None
        self._bindings = {b.library_id: b for b in AVAILABLE_BINDINGS}

    def ping(self) -> Dict[str, Any]:
        return {
            "alive": True,
            "uptime_ms": int((time.time() - self._start_time) * 1000),
            "memory_mb": self._get_memory_mb()
        }

    def get_info(self) -> Dict[str, Any]:
        import sys
        import platform

        return {
            "adapter": {
                "name": "ups-python-adapter",
                "version": "1.0.0",
                "protocol_version": "1.0"
            },
            "runtime": {
                "language": "python",
                "version": sys.version.split()[0],
                "platform": platform.platform()
            },
            "supported_libraries": [
                {
                    "id": b.library_id,
                    "name": b.library_name,
                    "version": b.library_version,
                    "formats": b.supported_formats,
                    "capabilities": b.capabilities
                }
                for b in self._bindings.values()
            ],
            "capabilities": ["streaming", "memory_tracking"]
        }

    def initialize(
        self,
        library_id: str,
        options: Optional[Dict[str, Any]] = None
    ) -> Dict[str, Any]:
        if library_id not in self._bindings:
            raise ValueError(f"Unknown library: {library_id}")

        self._current_binding = self._bindings[library_id]
        self._current_binding.initialize(options or {})

        return {
            "success": True,
            "library": {
                "id": self._current_binding.library_id,
                "name": self._current_binding.library_name,
                "version": self._current_binding.library_version
            }
        }

    def parse(
        self,
        input: Dict[str, Any],
        format: str,
        mode: str = "auto",
        options: Optional[Dict[str, Any]] = None,
        collect_metrics: bool = True
    ) -> Dict[str, Any]:
        if self._current_binding is None:
            raise RuntimeError("No library initialized")

        # Get input data
        data = self._resolve_input(input)

        # Collect metrics if requested
        if collect_metrics:
            tracemalloc.start()
            start_time = time.perf_counter_ns()

        try:
            output = self._current_binding.parse(
                data, format, options or {}
            )
            success = True
            errors = []
        except Exception as e:
            output = None
            success = False
            errors = [{"message": str(e), "code": "PARSE_ERROR"}]

        result = {
            "success": success,
            "output": output,
            "errors": errors,
            "warnings": []
        }

        if collect_metrics:
            duration_ns = time.perf_counter_ns() - start_time
            current, peak = tracemalloc.get_traced_memory()
            tracemalloc.stop()

            result["metrics"] = {
                "duration_ns": duration_ns,
                "memory_bytes": current,
                "peak_memory_bytes": peak
            }

        return result

    def shutdown(self) -> Dict[str, Any]:
        return {"success": True}

    def _resolve_input(self, input_spec: Dict[str, Any]) -> bytes:
        input_type = input_spec.get("type", "inline")

        if input_type == "inline":
            data = input_spec["data"]
            encoding = input_spec.get("encoding", "utf-8")
            if isinstance(data, str):
                return data.encode(encoding)
            return data

        elif input_type == "inline_base64":
            import base64
            return base64.b64decode(input_spec["data"])

        elif input_type == "file":
            with open(input_spec["path"], "rb") as f:
                return f.read()

        raise ValueError(f"Unknown input type: {input_type}")

    def _get_memory_mb(self) -> float:
        import resource
        return resource.getrusage(resource.RUSAGE_SELF).ru_maxrss / 1024
```

---

## 10. Testing Adapters

### 10.1 Adapter Test Suite

```yaml
# adapter-test-suite.yaml
tests:
  - name: "ping responds"
    request:
      method: ping
      params: {}
    expect:
      result.alive: true

  - name: "get_info returns valid structure"
    request:
      method: get_info
      params: {}
    expect:
      result.adapter.name: exists
      result.runtime.language: exists
      result.supported_libraries: is_array

  - name: "initialize succeeds with valid library"
    request:
      method: initialize
      params:
        library_id: "{{LIBRARY_ID}}"
    expect:
      result.success: true

  - name: "parse simple CSV"
    setup:
      - method: initialize
        params: { library_id: "{{LIBRARY_ID}}" }
    request:
      method: parse
      params:
        input:
          type: inline
          data: "a,b\n1,2"
        format: csv
        options:
          has_header: true
    expect:
      result.success: true
      result.output.headers: ["a", "b"]
      result.output.rowCount: 1
```

---

## 11. Appendix: Full Method Reference

| Method | Required | Description |
|--------|----------|-------------|
| `ping` | Yes | Health check |
| `get_info` | Yes | Adapter metadata |
| `initialize` | Yes | Load library |
| `parse` | Yes | Parse input |
| `parse_file` | No | Parse from file |
| `validate` | No | Validate input |
| `serialize` | No | Output to format |
| `get_options` | No | Get available options |
| `stream_start` | No | Start streaming |
| `stream_chunk` | No | Send stream chunk |
| `stream_result` | No | Get stream result |
| `shutdown` | Yes | Graceful shutdown |

---

*End of Universal Adapter Interface Specification*
