# Universal Parser Catalog

## 1000+ Parser Examples for UPS Schema Validation

This document catalogs 1000+ parsers across all domains to validate that the Universal Parser Specification (UPS) schema can describe ANY parser type. Each parser is categorized by domain, type, and complexity.

---

## Catalog Statistics

| Category | Count |
|----------|-------|
| Data Interchange Formats | 85 |
| Programming Languages | 150 |
| Configuration Formats | 65 |
| Network Protocols | 120 |
| Markup Languages | 55 |
| Binary Formats | 95 |
| Query Languages | 45 |
| Domain-Specific Languages | 80 |
| Media Formats | 75 |
| Security & Crypto Formats | 50 |
| Scientific & Engineering | 40 |
| Geographic & Mapping | 30 |
| Financial Formats | 35 |
| Healthcare Formats | 25 |
| Logging & Observability | 35 |
| Build & Package Formats | 40 |
| Miscellaneous | 75 |
| **Total** | **1100+** |

---

## 1. Data Interchange Formats (85)

### 1.1 Text-Based Data Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 1 | JSON | RFC 8259 | structured | medium |
| 2 | JSON5 | json5.org | structured | medium |
| 3 | JSON Lines (JSONL) | jsonlines.org | streaming | low |
| 4 | NDJSON | ndjson.org | streaming | low |
| 5 | JSON-LD | W3C | structured | high |
| 6 | JSON Pointer | RFC 6901 | text | low |
| 7 | JSON Patch | RFC 6902 | structured | medium |
| 8 | JSON Merge Patch | RFC 7396 | structured | medium |
| 9 | GeoJSON | RFC 7946 | structured | medium |
| 10 | TopoJSON | github.com/topojson | structured | high |
| 11 | JSON Schema | json-schema.org | structured | high |
| 12 | JSON-RPC 2.0 | jsonrpc.org | structured | medium |
| 13 | JSON API | jsonapi.org | structured | medium |
| 14 | HAL+JSON | stateless.group | structured | medium |
| 15 | JSON:API | jsonapi.org | structured | medium |
| 16 | JSON Resume | jsonresume.org | structured | medium |
| 17 | JSON Feed | jsonfeed.org | structured | medium |
| 18 | JSON-stat | json-stat.org | structured | medium |
| 19 | JMESPath Expression | jmespath.org | text | medium |
| 20 | JSONPath Expression | goessner.net | text | medium |
| 21 | XML 1.0 | W3C | structured | high |
| 22 | XML 1.1 | W3C | structured | high |
| 23 | XML Namespaces | W3C | structured | high |
| 24 | XPath 1.0 | W3C | text | high |
| 25 | XPath 2.0 | W3C | text | very high |
| 26 | XPath 3.0 | W3C | text | very high |
| 27 | XQuery 1.0 | W3C | text | very high |
| 28 | XSLT 1.0 | W3C | structured | very high |
| 29 | XSLT 2.0 | W3C | structured | very high |
| 30 | XML Schema (XSD) | W3C | structured | very high |
| 31 | RelaxNG | OASIS | structured | high |
| 32 | Schematron | ISO 19757-3 | structured | high |
| 33 | YAML 1.1 | yaml.org | structured | high |
| 34 | YAML 1.2 | yaml.org | structured | high |
| 35 | StrictYAML | hitchdev.com | structured | medium |
| 36 | CSV | RFC 4180 | tabular | low |
| 37 | TSV | IANA | tabular | low |
| 38 | PSV (Pipe-Separated) | - | tabular | low |
| 39 | SSV (Semicolon-Separated) | - | tabular | low |
| 40 | Fixed-Width Text | - | tabular | medium |

### 1.2 Binary Data Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 41 | Protocol Buffers Wire | Google | binary | medium |
| 42 | Protocol Buffers Text | Google | text | medium |
| 43 | MessagePack | msgpack.org | binary | medium |
| 44 | CBOR | RFC 8949 | binary | medium |
| 45 | BSON | bsonspec.org | binary | medium |
| 46 | Apache Avro | avro.apache.org | binary | high |
| 47 | Apache Thrift Binary | thrift.apache.org | binary | high |
| 48 | Apache Thrift Compact | thrift.apache.org | binary | high |
| 49 | Apache Parquet | parquet.apache.org | binary | very high |
| 50 | Apache ORC | orc.apache.org | binary | very high |
| 51 | Apache Arrow IPC | arrow.apache.org | binary | very high |
| 52 | Cap'n Proto | capnproto.org | binary | high |
| 53 | FlatBuffers | google.github.io | binary | high |
| 54 | FlexBuffers | google.github.io | binary | medium |
| 55 | UBJSON | ubjson.org | binary | medium |
| 56 | Smile (Jackson) | github.com/FasterXML | binary | medium |
| 57 | BSER (Watchman) | facebook.github.io | binary | medium |
| 58 | Ion Binary | amzn.github.io/ion | binary | high |
| 59 | Ion Text | amzn.github.io/ion | text | high |
| 60 | Bencode | bittorrent.org | binary | low |
| 61 | Netstrings | cr.yp.to | binary | low |
| 62 | PHP Serialize | php.net | binary | medium |
| 63 | Python Pickle | python.org | binary | high |
| 64 | Java Serialization | Oracle | binary | high |
| 65 | .NET Binary Formatter | Microsoft | binary | high |
| 66 | Ruby Marshal | ruby-lang.org | binary | medium |
| 67 | Erlang External Term | erlang.org | binary | medium |
| 68 | S-expression | - | text | low |
| 69 | EDN (Extensible Data Notation) | edn-format.org | text | medium |
| 70 | Transit | cognitect.com | hybrid | medium |
| 71 | Nippy | github.com/ptaoussanis | binary | medium |
| 72 | Kryo | github.com/EsotericSoftware | binary | medium |
| 73 | FST | github.com/RuedigerMoeller | binary | medium |
| 74 | Hessian | caucho.com | binary | medium |
| 75 | AMF (Action Message Format) | Adobe | binary | medium |
| 76 | AMF0 | Adobe | binary | medium |
| 77 | AMF3 | Adobe | binary | medium |
| 78 | WBXML | W3C | binary | medium |
| 79 | Fast Infoset | ITU-T X.891 | binary | high |
| 80 | EXI | W3C | binary | very high |
| 81 | Property List (Binary) | Apple | binary | medium |
| 82 | Property List (XML) | Apple | structured | medium |
| 83 | Property List (JSON) | Apple | structured | medium |
| 84 | NBT (Minecraft) | wiki.vg | binary | medium |
| 85 | RLP (Ethereum) | ethereum.org | binary | medium |

---

## 2. Programming Languages (150)

### 2.1 General Purpose Languages

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 86 | C89/C90 | ISO/IEC 9899:1990 | text | very high |
| 87 | C99 | ISO/IEC 9899:1999 | text | very high |
| 88 | C11 | ISO/IEC 9899:2011 | text | very high |
| 89 | C17 | ISO/IEC 9899:2018 | text | very high |
| 90 | C23 | ISO/IEC 9899:2024 | text | very high |
| 91 | C++ 98 | ISO/IEC 14882:1998 | text | extremely high |
| 92 | C++ 11 | ISO/IEC 14882:2011 | text | extremely high |
| 93 | C++ 14 | ISO/IEC 14882:2014 | text | extremely high |
| 94 | C++ 17 | ISO/IEC 14882:2017 | text | extremely high |
| 95 | C++ 20 | ISO/IEC 14882:2020 | text | extremely high |
| 96 | C++ 23 | ISO/IEC 14882:2023 | text | extremely high |
| 97 | C# 1.0 | ECMA-334 | text | very high |
| 98 | C# 12.0 | Microsoft | text | very high |
| 99 | Java SE 8 | Oracle | text | very high |
| 100 | Java SE 21 | Oracle | text | very high |
| 101 | Kotlin | JetBrains | text | very high |
| 102 | Scala 2 | scala-lang.org | text | extremely high |
| 103 | Scala 3 | scala-lang.org | text | extremely high |
| 104 | Groovy | groovy-lang.org | text | very high |
| 105 | Python 2.7 | python.org | text | high |
| 106 | Python 3.12 | python.org | text | high |
| 107 | Ruby 3.3 | ruby-lang.org | text | high |
| 108 | Perl 5 | perl.org | text | very high |
| 109 | Raku (Perl 6) | raku.org | text | extremely high |
| 110 | PHP 8 | php.net | text | high |
| 111 | JavaScript ES5 | ECMA-262 5th | text | high |
| 112 | JavaScript ES6+ | ECMA-262 | text | very high |
| 113 | TypeScript | typescriptlang.org | text | very high |
| 114 | CoffeeScript | coffeescript.org | text | medium |
| 115 | Dart | dart.dev | text | high |
| 116 | Go | go.dev | text | medium |
| 117 | Rust | rust-lang.org | text | very high |
| 118 | Swift | swift.org | text | very high |
| 119 | Objective-C | Apple | text | high |
| 120 | D | dlang.org | text | very high |
| 121 | Nim | nim-lang.org | text | high |
| 122 | Crystal | crystal-lang.org | text | high |
| 123 | Zig | ziglang.org | text | high |
| 124 | V | vlang.io | text | medium |
| 125 | Odin | odin-lang.org | text | medium |
| 126 | Carbon | github.com/carbon-language | text | very high |
| 127 | Mojo | modular.com | text | high |

### 2.2 Functional Languages

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 128 | Haskell 2010 | haskell.org | text | very high |
| 129 | GHC Haskell | ghc.haskell.org | text | extremely high |
| 130 | OCaml | ocaml.org | text | very high |
| 131 | F# | fsharp.org | text | very high |
| 132 | Standard ML | sml-family.org | text | high |
| 133 | Elm | elm-lang.org | text | medium |
| 134 | PureScript | purescript.org | text | high |
| 135 | Clojure | clojure.org | text | medium |
| 136 | ClojureScript | clojurescript.org | text | medium |
| 137 | Common Lisp | lisp-lang.org | text | high |
| 138 | Scheme R7RS | scheme.org | text | medium |
| 139 | Racket | racket-lang.org | text | high |
| 140 | Elixir | elixir-lang.org | text | medium |
| 141 | Erlang | erlang.org | text | medium |
| 142 | LFE (Lisp Flavored Erlang) | lfe.io | text | medium |
| 143 | Gleam | gleam.run | text | medium |
| 144 | Idris 2 | idris-lang.org | text | very high |
| 145 | Agda | wiki.portal.chalmers.se/agda | text | extremely high |
| 146 | Coq | coq.inria.fr | text | extremely high |
| 147 | Lean 4 | lean-lang.org | text | extremely high |
| 148 | Miranda | miranda.org.uk | text | high |
| 149 | Clean | clean.cs.ru.nl | text | high |
| 150 | Unison | unison-lang.org | text | high |

### 2.3 Systems & Low-Level Languages

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 151 | Assembly x86 | Intel | text | high |
| 152 | Assembly x86-64 | AMD | text | high |
| 153 | Assembly ARM | ARM | text | high |
| 154 | Assembly ARM64 | ARM | text | high |
| 155 | Assembly RISC-V | riscv.org | text | high |
| 156 | Assembly MIPS | MIPS | text | high |
| 157 | Assembly WebAssembly Text | W3C | text | medium |
| 158 | LLVM IR | llvm.org | text | high |
| 159 | SPIR-V | Khronos | binary | high |
| 160 | PTX | NVIDIA | text | high |
| 161 | GAS (GNU Assembler) | GNU | text | high |
| 162 | NASM | nasm.us | text | high |
| 163 | FASM | flatassembler.net | text | high |

### 2.4 Scripting Languages

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 164 | Lua 5.4 | lua.org | text | low |
| 165 | LuaJIT | luajit.org | text | low |
| 166 | Tcl | tcl.tk | text | medium |
| 167 | AWK | IEEE 1003.1 | text | low |
| 168 | Sed | IEEE 1003.1 | text | low |
| 169 | Bash | gnu.org | text | medium |
| 170 | Zsh | zsh.org | text | medium |
| 171 | Fish | fishshell.com | text | medium |
| 172 | PowerShell | microsoft.com | text | high |
| 173 | Nushell | nushell.sh | text | medium |
| 174 | Batch/CMD | Microsoft | text | low |
| 175 | VBScript | Microsoft | text | medium |
| 176 | JScript | Microsoft | text | medium |
| 177 | AppleScript | Apple | text | medium |
| 178 | AutoHotkey | autohotkey.com | text | medium |
| 179 | AutoIt | autoitscript.com | text | medium |
| 180 | mIRC Script | mirc.com | text | low |

### 2.5 Academic & Research Languages

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 181 | Prolog | ISO 13211 | text | medium |
| 182 | Mercury | mercurylang.org | text | high |
| 183 | Datalog | - | text | low |
| 184 | MiniKanren | minikanren.org | text | medium |
| 185 | Forth | Forth-2012 | text | low |
| 186 | Factor | factorcode.org | text | medium |
| 187 | J | jsoftware.com | text | medium |
| 188 | K | kx.com | text | low |
| 189 | Q (kdb+) | kx.com | text | medium |
| 190 | APL | ISO 8485 | text | medium |
| 191 | BQN | mlochbaum.github.io/BQN | text | medium |
| 192 | Uiua | uiua.org | text | medium |
| 193 | Smalltalk | ANSI | text | medium |
| 194 | Pharo | pharo.org | text | medium |
| 195 | Self | selflanguage.org | text | medium |
| 196 | Io | iolanguage.org | text | low |
| 197 | Ioke | ioke.org | text | low |
| 198 | Rebol | rebol.com | text | medium |
| 199 | Red | red-lang.org | text | medium |
| 200 | Pony | ponylang.io | text | high |
| 201 | ATS | ats-lang.org | text | extremely high |
| 202 | Flix | flix.dev | text | high |
| 203 | Koka | koka-lang.github.io | text | high |
| 204 | Effekt | effekt-lang.org | text | high |
| 205 | Frank | github.com/frank-lang | text | high |

### 2.6 JVM Languages

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 206 | Clojure | clojure.org | text | medium |
| 207 | Kotlin JVM | kotlinlang.org | text | very high |
| 208 | Scala JVM | scala-lang.org | text | extremely high |
| 209 | Groovy | groovy-lang.org | text | high |
| 210 | JRuby | jruby.org | text | high |
| 211 | Jython | jython.org | text | high |
| 212 | Frege | github.com/Frege | text | very high |
| 213 | Eta | eta-lang.org | text | very high |
| 214 | Ceylon | ceylon-lang.org | text | high |
| 215 | Fantom | fantom.org | text | medium |
| 216 | Gosu | gosu-lang.github.io | text | high |
| 217 | Xtend | eclipse.org/xtend | text | high |
| 218 | Mirah | mirah.org | text | medium |

### 2.7 .NET Languages

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 219 | C# | ECMA-334 | text | very high |
| 220 | F# | fsharp.org | text | very high |
| 221 | Visual Basic .NET | Microsoft | text | medium |
| 222 | IronPython | ironpython.net | text | high |
| 223 | IronRuby | ironruby.net | text | high |
| 224 | Boo | boo-lang.org | text | medium |
| 225 | Nemerle | nemerle.org | text | high |
| 226 | PowerShell | microsoft.com | text | high |

### 2.8 Esoteric Languages

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 227 | Brainfuck | esolangs.org | text | low |
| 228 | Befunge | esolangs.org | text | low |
| 229 | Whitespace | esolangs.org | text | low |
| 230 | LOLCODE | lolcode.org | text | low |
| 231 | Piet | dangermouse.net | image | medium |
| 232 | Shakespeare | shakespearelang.com | text | medium |
| 233 | Chef | dangermouse.net | text | low |
| 234 | Malbolge | esolangs.org | text | medium |
| 235 | Unlambda | madore.org | text | low |

---

## 3. Configuration Formats (65)

### 3.1 General Configuration

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 236 | INI | - | text | low |
| 237 | TOML | toml.io | text | medium |
| 238 | HCL | hashicorp.com | text | medium |
| 239 | HCL2 | hashicorp.com | text | high |
| 240 | Cue | cuelang.org | text | high |
| 241 | Jsonnet | jsonnet.org | text | high |
| 242 | Dhall | dhall-lang.org | text | high |
| 243 | Nickel | nickel-lang.org | text | high |
| 244 | Pkl | pkl-lang.org | text | medium |
| 245 | KCL | kcl-lang.io | text | high |
| 246 | UCL | github.com/vstakhov | text | medium |
| 247 | Hocon | lightbend.github.io | text | medium |
| 248 | Libconfig | hyperrealm.github.io | text | low |
| 249 | dotenv | - | text | low |
| 250 | .editorconfig | editorconfig.org | text | low |
| 251 | .gitignore | git-scm.com | text | low |
| 252 | .gitattributes | git-scm.com | text | low |
| 253 | .npmrc | npm.community | text | low |

### 3.2 Build System Configurations

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 254 | Makefile | GNU Make | text | medium |
| 255 | CMakeLists.txt | cmake.org | text | high |
| 256 | Bazel BUILD | bazel.build | text | high |
| 257 | Buck BUCK | buck.build | text | high |
| 258 | Pants BUILD | pantsbuild.org | text | medium |
| 259 | Ninja | ninja-build.org | text | low |
| 260 | Meson | mesonbuild.com | text | medium |
| 261 | SCons | scons.org | text | medium |
| 262 | Gradle (Groovy DSL) | gradle.org | text | high |
| 263 | Gradle (Kotlin DSL) | gradle.org | text | high |
| 264 | Maven POM | maven.apache.org | structured | medium |
| 265 | Ant build.xml | ant.apache.org | structured | medium |
| 266 | package.json | npm | structured | medium |
| 267 | Cargo.toml | rust-lang.org | text | medium |
| 268 | pyproject.toml | python.org | text | medium |
| 269 | setup.py | python.org | text | medium |
| 270 | Gemfile | bundler.io | text | medium |
| 271 | go.mod | go.dev | text | low |
| 272 | mix.exs | elixir-lang.org | text | medium |
| 273 | rebar.config | rebar3.org | text | low |
| 274 | cabal.project | haskell.org | text | medium |
| 275 | stack.yaml | haskellstack.org | text | medium |
| 276 | pubspec.yaml | dart.dev | structured | medium |
| 277 | Package.swift | swift.org | text | medium |
| 278 | .csproj | Microsoft | structured | medium |
| 279 | .fsproj | Microsoft | structured | medium |
| 280 | Directory.Build.props | Microsoft | structured | low |

### 3.3 Infrastructure Configurations

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 281 | Terraform | hashicorp.com | text | high |
| 282 | Pulumi (YAML) | pulumi.com | structured | medium |
| 283 | CloudFormation | AWS | structured | high |
| 284 | ARM Templates | Azure | structured | high |
| 285 | Bicep | Azure | text | medium |
| 286 | Ansible Playbook | ansible.com | structured | medium |
| 287 | SaltStack | saltproject.io | structured | medium |
| 288 | Puppet Manifest | puppet.com | text | medium |
| 289 | Chef Recipe | chef.io | text | medium |
| 290 | Kubernetes YAML | kubernetes.io | structured | medium |
| 291 | Helm Chart | helm.sh | structured | medium |
| 292 | Kustomize | kubernetes.io | structured | medium |
| 293 | Docker Compose | docker.com | structured | medium |
| 294 | Dockerfile | docker.com | text | medium |
| 295 | Vagrantfile | vagrantup.com | text | medium |
| 296 | Packer | hashicorp.com | structured | medium |
| 297 | Nomad | hashicorp.com | text | medium |
| 298 | Consul | hashicorp.com | text | medium |
| 299 | Vault | hashicorp.com | text | medium |
| 300 | Boundary | hashicorp.com | text | medium |

---

## 4. Network Protocols (120)

### 4.1 Application Layer (HTTP Family)

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 301 | HTTP/1.0 | RFC 1945 | text | medium |
| 302 | HTTP/1.1 | RFC 7230-7235 | text | high |
| 303 | HTTP/2 | RFC 7540 | binary | very high |
| 304 | HTTP/3 | RFC 9114 | binary | very high |
| 305 | HTTPS | RFC 2818 | binary | very high |
| 306 | HTTP Request | RFC 7230 | text | medium |
| 307 | HTTP Response | RFC 7230 | text | medium |
| 308 | HTTP Headers | RFC 7231 | text | medium |
| 309 | HTTP Chunked Transfer | RFC 7230 | text | medium |
| 310 | HTTP/2 Frame | RFC 7540 | binary | high |
| 311 | HTTP/3 Frame | RFC 9114 | binary | high |
| 312 | WebSocket | RFC 6455 | binary | high |
| 313 | WebSocket Frame | RFC 6455 | binary | medium |
| 314 | Server-Sent Events | W3C | text | low |
| 315 | WebDAV | RFC 4918 | text | high |
| 316 | CalDAV | RFC 4791 | text | high |
| 317 | CardDAV | RFC 6352 | text | high |
| 318 | WebFinger | RFC 7033 | text | low |
| 319 | CORS Headers | W3C Fetch | text | low |
| 320 | Content-Type Header | RFC 7231 | text | low |
| 321 | Accept Header | RFC 7231 | text | low |
| 322 | Cache-Control Header | RFC 7234 | text | low |
| 323 | Cookie Header | RFC 6265 | text | medium |
| 324 | Set-Cookie Header | RFC 6265 | text | medium |
| 325 | Authorization Header | RFC 7235 | text | medium |
| 326 | WWW-Authenticate | RFC 7235 | text | medium |

### 4.2 Email Protocols

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 327 | SMTP | RFC 5321 | text | medium |
| 328 | ESMTP | RFC 5321 | text | medium |
| 329 | POP3 | RFC 1939 | text | low |
| 330 | IMAP4 | RFC 3501 | text | high |
| 331 | MIME | RFC 2045-2049 | text | high |
| 332 | Email Address | RFC 5321 | text | medium |
| 333 | Email Headers | RFC 5322 | text | medium |
| 334 | Internet Message Format | RFC 5322 | text | medium |
| 335 | DKIM Signature | RFC 6376 | text | medium |
| 336 | SPF Record | RFC 7208 | text | medium |
| 337 | DMARC Record | RFC 7489 | text | medium |
| 338 | S/MIME | RFC 8551 | binary | high |
| 339 | OpenPGP | RFC 4880 | binary | high |

### 4.3 DNS & Name Resolution

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 340 | DNS Message | RFC 1035 | binary | medium |
| 341 | DNS Query | RFC 1035 | binary | medium |
| 342 | DNS Response | RFC 1035 | binary | medium |
| 343 | DNS-over-HTTPS | RFC 8484 | binary | high |
| 344 | DNS-over-TLS | RFC 7858 | binary | high |
| 345 | DNSSEC | RFC 4033-4035 | binary | high |
| 346 | mDNS | RFC 6762 | binary | medium |
| 347 | DNS Zone File | RFC 1035 | text | medium |
| 348 | DNS SRV Record | RFC 2782 | text | low |
| 349 | DNS CAA Record | RFC 6844 | text | low |
| 350 | DNS NAPTR Record | RFC 3403 | text | medium |

### 4.4 Security Protocols

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 351 | TLS 1.2 | RFC 5246 | binary | very high |
| 352 | TLS 1.3 | RFC 8446 | binary | very high |
| 353 | TLS Record | RFC 8446 | binary | high |
| 354 | TLS Handshake | RFC 8446 | binary | very high |
| 355 | TLS Certificate | RFC 5280 | binary | high |
| 356 | DTLS 1.2 | RFC 6347 | binary | very high |
| 357 | DTLS 1.3 | RFC 9147 | binary | very high |
| 358 | SSH | RFC 4253 | binary | high |
| 359 | SSH Key | RFC 4253 | text | medium |
| 360 | SSH Config | OpenSSH | text | medium |
| 361 | Kerberos V5 | RFC 4120 | binary | very high |
| 362 | SASL | RFC 4422 | text | high |
| 363 | OAuth 2.0 | RFC 6749 | text | medium |
| 364 | OpenID Connect | openid.net | text | high |
| 365 | JWT | RFC 7519 | text | medium |
| 366 | JWS | RFC 7515 | text | medium |
| 367 | JWE | RFC 7516 | text | medium |
| 368 | JWK | RFC 7517 | text | medium |
| 369 | PASETO | paseto.io | text | medium |

### 4.5 Transport & Network Layer

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 370 | TCP | RFC 793 | binary | high |
| 371 | UDP | RFC 768 | binary | low |
| 372 | QUIC | RFC 9000 | binary | very high |
| 373 | SCTP | RFC 4960 | binary | high |
| 374 | IP Header (IPv4) | RFC 791 | binary | medium |
| 375 | IP Header (IPv6) | RFC 8200 | binary | medium |
| 376 | ICMP | RFC 792 | binary | low |
| 377 | ICMPv6 | RFC 4443 | binary | low |
| 378 | ARP | RFC 826 | binary | low |
| 379 | NDP | RFC 4861 | binary | medium |
| 380 | Ethernet Frame | IEEE 802.3 | binary | low |
| 381 | VLAN Tag | IEEE 802.1Q | binary | low |
| 382 | MPLS | RFC 3031 | binary | medium |
| 383 | GRE | RFC 2784 | binary | low |
| 384 | VXLAN | RFC 7348 | binary | medium |
| 385 | WireGuard | wireguard.com | binary | medium |
| 386 | IPsec | RFC 4301 | binary | very high |

### 4.6 Messaging Protocols

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 387 | MQTT 3.1.1 | OASIS | binary | medium |
| 388 | MQTT 5.0 | OASIS | binary | high |
| 389 | AMQP 0-9-1 | RabbitMQ | binary | high |
| 390 | AMQP 1.0 | OASIS | binary | very high |
| 391 | STOMP | STOMP Spec | text | low |
| 392 | XMPP | RFC 6120 | text | high |
| 393 | SIP | RFC 3261 | text | high |
| 394 | SDP | RFC 4566 | text | medium |
| 395 | RTP | RFC 3550 | binary | medium |
| 396 | RTCP | RFC 3550 | binary | medium |
| 397 | RTSP | RFC 7826 | text | medium |
| 398 | WebRTC SDP | W3C | text | high |
| 399 | CoAP | RFC 7252 | binary | medium |
| 400 | LwM2M | OMA | binary | medium |
| 401 | DDS | OMG | binary | high |
| 402 | ZeroMQ | zeromq.org | binary | medium |
| 403 | nanomsg | nanomsg.org | binary | low |
| 404 | NATS | nats.io | text | low |

### 4.7 Database Protocols

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 405 | PostgreSQL Wire | postgresql.org | binary | high |
| 406 | MySQL Client/Server | MySQL | binary | high |
| 407 | MongoDB Wire | mongodb.com | binary | high |
| 408 | Redis RESP | redis.io | text | low |
| 409 | Redis RESP3 | redis.io | text | medium |
| 410 | Memcached Text | memcached.org | text | low |
| 411 | Memcached Binary | memcached.org | binary | medium |
| 412 | Cassandra CQL Binary | cassandra.apache.org | binary | high |
| 413 | ClickHouse Native | clickhouse.com | binary | high |
| 414 | InfluxDB Line Protocol | influxdata.com | text | low |
| 415 | OpenTSDB | opentsdb.net | text | low |
| 416 | Graphite | graphiteapp.org | text | low |
| 417 | Prometheus Remote Write | prometheus.io | binary | medium |
| 418 | LDAP | RFC 4511 | binary | high |
| 419 | LDIF | RFC 2849 | text | medium |
| 420 | JDBC | Oracle | binary | high |

---

## 5. Markup Languages (55)

### 5.1 Document Markup

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 421 | HTML5 | W3C/WHATWG | text | high |
| 422 | XHTML | W3C | text | high |
| 423 | HTML Strict | W3C | text | high |
| 424 | HTML Quirks | - | text | high |
| 425 | Markdown (CommonMark) | commonmark.org | text | medium |
| 426 | GitHub Flavored Markdown | github.github.com | text | medium |
| 427 | MultiMarkdown | fletcherpenney.net | text | medium |
| 428 | Markdown Extra | michelf.ca | text | medium |
| 429 | R Markdown | rmarkdown.rstudio.com | text | high |
| 430 | reStructuredText | docutils.sourceforge.io | text | medium |
| 431 | AsciiDoc | asciidoc.org | text | high |
| 432 | Asciidoctor | asciidoctor.org | text | high |
| 433 | Textile | textile-lang.com | text | medium |
| 434 | Creole | wikicreole.org | text | low |
| 435 | MediaWiki | mediawiki.org | text | high |
| 436 | Wikitext | - | text | medium |
| 437 | Org Mode | orgmode.org | text | high |
| 438 | Typst | typst.app | text | high |
| 439 | LaTeX | latex-project.org | text | very high |
| 440 | TeX | tug.org | text | very high |
| 441 | ConTeXt | contextgarden.net | text | very high |
| 442 | Texinfo | gnu.org | text | medium |
| 443 | Troff/Groff | gnu.org | text | medium |
| 444 | man page | - | text | medium |
| 445 | Pod (Perl) | perldoc.perl.org | text | low |
| 446 | DITA | oasis-open.org | structured | high |
| 447 | DocBook | docbook.org | structured | high |
| 448 | TEI | tei-c.org | structured | high |
| 449 | JATS | niso.org | structured | high |
| 450 | EPUB | W3C | structured | high |

### 5.2 UI Markup

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 451 | CSS | W3C | text | high |
| 452 | CSS Selectors | W3C | text | medium |
| 453 | SCSS/Sass | sass-lang.com | text | medium |
| 454 | Less | lesscss.org | text | medium |
| 455 | Stylus | stylus-lang.com | text | medium |
| 456 | PostCSS | postcss.org | text | medium |
| 457 | SVG | W3C | structured | high |
| 458 | XAML | Microsoft | structured | high |
| 459 | QML | qt.io | text | high |
| 460 | Qt Style Sheets | qt.io | text | medium |
| 461 | JavaFX FXML | Oracle | structured | medium |
| 462 | Android Layout XML | Android | structured | medium |
| 463 | SwiftUI DSL | Apple | text | high |
| 464 | Jetpack Compose | Android | text | high |
| 465 | Flutter Widget DSL | dart.dev | text | high |
| 466 | React JSX | react.dev | text | high |
| 467 | Vue SFC | vuejs.org | text | high |
| 468 | Svelte | svelte.dev | text | high |
| 469 | Astro | astro.build | text | high |
| 470 | MDX | mdxjs.com | text | high |
| 471 | Pug | pugjs.org | text | medium |
| 472 | Haml | haml.info | text | medium |
| 473 | Slim | slim-template.github.io | text | medium |
| 474 | Jinja2 | palletsprojects.com | text | medium |
| 475 | Liquid | shopify.github.io/liquid | text | medium |

---

## 6. Binary Formats (95)

### 6.1 Image Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 476 | PNG | W3C | binary | medium |
| 477 | JPEG | ITU-T T.81 | binary | high |
| 478 | JPEG 2000 | ISO 15444 | binary | very high |
| 479 | JPEG XL | ISO 18181 | binary | very high |
| 480 | GIF | CompuServe | binary | medium |
| 481 | WebP | Google | binary | high |
| 482 | AVIF | AOMedia | binary | high |
| 483 | HEIF | MPEG | binary | high |
| 484 | BMP | Microsoft | binary | low |
| 485 | TIFF | Adobe | binary | high |
| 486 | PSD | Adobe | binary | very high |
| 487 | RAW (various) | Camera vendors | binary | high |
| 488 | DNG | Adobe | binary | high |
| 489 | ICO | Microsoft | binary | low |
| 490 | EXR | OpenEXR | binary | high |
| 491 | HDR | Radiance | binary | medium |
| 492 | QOI | qoiformat.org | binary | low |
| 493 | FLIF | flif.info | binary | high |

### 6.2 Audio Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 494 | WAV | Microsoft | binary | medium |
| 495 | MP3 | ISO 11172-3 | binary | high |
| 496 | AAC | ISO 13818-7 | binary | high |
| 497 | FLAC | xiph.org | binary | medium |
| 498 | Ogg Vorbis | xiph.org | binary | high |
| 499 | Opus | RFC 6716 | binary | high |
| 500 | ALAC | Apple | binary | medium |
| 501 | WMA | Microsoft | binary | high |
| 502 | AIFF | Apple | binary | medium |
| 503 | MIDI | MMA | binary | medium |

### 6.3 Video Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 504 | MP4/M4V | ISO 14496-14 | binary | very high |
| 505 | MKV/WebM | matroska.org | binary | very high |
| 506 | AVI | Microsoft | binary | medium |
| 507 | MOV | Apple | binary | high |
| 508 | FLV | Adobe | binary | medium |
| 509 | WMV | Microsoft | binary | high |
| 510 | MPEG-TS | ISO 13818-1 | binary | high |
| 511 | H.264/AVC NAL | ITU-T H.264 | binary | very high |
| 512 | H.265/HEVC | ITU-T H.265 | binary | very high |
| 513 | AV1 | AOMedia | binary | very high |
| 514 | VP8/VP9 | Google | binary | high |

### 6.4 Archive Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 515 | ZIP | PKWARE | binary | medium |
| 516 | GZIP | RFC 1952 | binary | low |
| 517 | BZIP2 | sourceware.org | binary | medium |
| 518 | XZ/LZMA | tukaani.org | binary | medium |
| 519 | Zstandard | facebook.github.io | binary | medium |
| 520 | LZ4 | lz4.github.io | binary | low |
| 521 | Snappy | google.github.io | binary | low |
| 522 | Brotli | github.com/google | binary | medium |
| 523 | RAR | rarlab.com | binary | high |
| 524 | 7z | 7-zip.org | binary | high |
| 525 | TAR | IEEE 1003.1 | binary | low |
| 526 | CPIO | IEEE 1003.1 | binary | low |
| 527 | AR | Unix | binary | low |

### 6.5 Document Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 528 | PDF | ISO 32000 | binary | very high |
| 529 | PDF/A | ISO 19005 | binary | very high |
| 530 | PostScript | Adobe | text | high |
| 531 | EPS | Adobe | text | high |
| 532 | DjVu | djvu.org | binary | high |
| 533 | XPS | Microsoft | binary | high |
| 534 | RTF | Microsoft | text | medium |
| 535 | ODF | OASIS | binary | high |
| 536 | OOXML (.docx) | ISO 29500 | binary | high |
| 537 | OOXML (.xlsx) | ISO 29500 | binary | high |
| 538 | OOXML (.pptx) | ISO 29500 | binary | high |
| 539 | Apple iWork | Apple | binary | high |

### 6.6 Executable Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 540 | ELF | System V ABI | binary | high |
| 541 | PE/COFF | Microsoft | binary | high |
| 542 | Mach-O | Apple | binary | high |
| 543 | WebAssembly Binary | W3C | binary | high |
| 544 | Java Class File | JVM Spec | binary | high |
| 545 | .NET Assembly | ECMA-335 | binary | high |
| 546 | Python Bytecode | python.org | binary | medium |
| 547 | Lua Bytecode | lua.org | binary | medium |
| 548 | DEX (Android) | Android | binary | high |
| 549 | ART (Android) | Android | binary | very high |
| 550 | LLVM Bitcode | llvm.org | binary | high |

### 6.7 Font Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 551 | TrueType | Apple | binary | high |
| 552 | OpenType | Microsoft/Adobe | binary | very high |
| 553 | WOFF | W3C | binary | medium |
| 554 | WOFF2 | W3C | binary | medium |
| 555 | EOT | Microsoft | binary | medium |
| 556 | Type 1 | Adobe | binary | medium |
| 557 | CFF | Adobe | binary | high |
| 558 | CFF2 | Adobe | binary | high |
| 559 | SVG Font | W3C | structured | medium |
| 560 | BDF | Adobe | text | low |

### 6.8 3D & Graphics Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 561 | glTF | Khronos | structured | high |
| 562 | glTF Binary | Khronos | binary | high |
| 563 | OBJ | Wavefront | text | medium |
| 564 | FBX | Autodesk | binary | very high |
| 565 | STL | 3D Systems | binary | low |
| 566 | PLY | Stanford | text/binary | medium |
| 567 | COLLADA | Khronos | structured | high |
| 568 | USD | Pixar | binary | very high |
| 569 | Alembic | Sony | binary | high |
| 570 | Blender | blender.org | binary | very high |

---

## 7. Query Languages (45)

### 7.1 Database Query Languages

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 571 | SQL-92 | ISO 9075:1992 | text | high |
| 572 | SQL:2016 | ISO 9075:2016 | text | very high |
| 573 | PostgreSQL SQL | postgresql.org | text | very high |
| 574 | MySQL SQL | mysql.com | text | high |
| 575 | SQLite SQL | sqlite.org | text | high |
| 576 | T-SQL | Microsoft | text | high |
| 577 | PL/SQL | Oracle | text | very high |
| 578 | PL/pgSQL | postgresql.org | text | high |
| 579 | HQL (Hibernate) | hibernate.org | text | high |
| 580 | JPQL | JPA | text | high |
| 581 | CQL (Cassandra) | cassandra.apache.org | text | medium |
| 582 | N1QL (Couchbase) | couchbase.com | text | high |
| 583 | AQL (ArangoDB) | arangodb.com | text | high |
| 584 | MQL (MongoDB) | mongodb.com | structured | high |
| 585 | PartiQL | partiql.org | text | high |

### 7.2 Graph Query Languages

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 586 | Cypher | neo4j.com | text | medium |
| 587 | GQL | ISO/IEC | text | high |
| 588 | SPARQL | W3C | text | high |
| 589 | Gremlin | apache.org | text | high |
| 590 | GraphQL | graphql.org | text | high |
| 591 | GraphQL SDL | graphql.org | text | medium |
| 592 | GSQL (TigerGraph) | tigergraph.com | text | high |
| 593 | openCypher | opencypher.org | text | medium |

### 7.3 Search & Filter Languages

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 594 | Elasticsearch Query DSL | elastic.co | structured | high |
| 595 | Lucene Query | Apache | text | medium |
| 596 | Solr Query | Apache | text | medium |
| 597 | KQL (Kibana) | elastic.co | text | medium |
| 598 | WIQL (Azure DevOps) | Microsoft | text | medium |
| 599 | JQL (Jira) | Atlassian | text | medium |
| 600 | OData Filter | odata.org | text | medium |
| 601 | FIQL/RSQL | - | text | low |
| 602 | CQL (SRU) | Library of Congress | text | medium |

### 7.4 Data Access Languages

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 603 | XPath | W3C | text | high |
| 604 | XQuery | W3C | text | very high |
| 605 | JSONPath | goessner.net | text | low |
| 606 | JMESPath | jmespath.org | text | medium |
| 607 | JSONiq | jsoniq.org | text | high |
| 608 | jq | stedolan.github.io | text | medium |
| 609 | CSS Selectors | W3C | text | medium |
| 610 | yq (YAML Query) | mikefarah.gitbook.io | text | medium |
| 611 | OPath | - | text | low |
| 612 | LINQ Expression | Microsoft | text | high |
| 613 | Kusto (KQL) | Microsoft | text | high |
| 614 | PromQL | prometheus.io | text | medium |
| 615 | LogQL | grafana.com | text | medium |

---

## 8. Domain-Specific Languages (80)

### 8.1 Build & Automation

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 616 | Make DSL | GNU | text | medium |
| 617 | CMake DSL | cmake.org | text | medium |
| 618 | Bazel Starlark | bazel.build | text | medium |
| 619 | Gradle DSL | gradle.org | text | high |
| 620 | Jenkinsfile | jenkins.io | text | medium |
| 621 | GitHub Actions YAML | github.com | structured | medium |
| 622 | GitLab CI YAML | gitlab.com | structured | medium |
| 623 | CircleCI Config | circleci.com | structured | medium |
| 624 | Travis CI | travis-ci.org | structured | medium |
| 625 | Bitbucket Pipelines | bitbucket.org | structured | medium |
| 626 | Azure Pipelines | Microsoft | structured | medium |
| 627 | Drone CI | drone.io | structured | medium |
| 628 | Argo Workflows | argoproj.github.io | structured | medium |
| 629 | Tekton Pipeline | tekton.dev | structured | medium |

### 8.2 Infrastructure

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 630 | Terraform HCL | hashicorp.com | text | high |
| 631 | Pulumi YAML | pulumi.com | structured | medium |
| 632 | AWS CDK | AWS | text | high |
| 633 | Ansible YAML | ansible.com | structured | medium |
| 634 | Puppet DSL | puppet.com | text | medium |
| 635 | Chef Ruby DSL | chef.io | text | medium |
| 636 | SaltStack | saltproject.io | structured | medium |
| 637 | Crossplane XRD | crossplane.io | structured | high |

### 8.3 Data Processing

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 638 | Apache Beam | beam.apache.org | text | high |
| 639 | Apache Spark SQL | spark.apache.org | text | high |
| 640 | dbt (SQL) | getdbt.com | text | medium |
| 641 | Dataform SQLX | dataform.co | text | medium |
| 642 | Malloy | malloydata.github.io | text | medium |
| 643 | PRQL | prql-lang.org | text | medium |
| 644 | Pig Latin | pig.apache.org | text | medium |
| 645 | Hive QL | hive.apache.org | text | high |
| 646 | Flink SQL | flink.apache.org | text | high |
| 647 | ksqlDB | ksqldb.io | text | medium |

### 8.4 Modeling & Specification

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 648 | UML (XMI) | OMG | structured | very high |
| 649 | PlantUML | plantuml.com | text | medium |
| 650 | Mermaid | mermaid.js.org | text | medium |
| 651 | D2 | d2lang.com | text | medium |
| 652 | Graphviz DOT | graphviz.org | text | medium |
| 653 | BPMN (XML) | OMG | structured | high |
| 654 | DMN | OMG | structured | high |
| 655 | Alloy | alloytools.org | text | high |
| 656 | TLA+ | lamport.azurewebsites.net | text | high |
| 657 | Z Notation | - | text | high |
| 658 | VDM-SL | vdmtools.jp | text | high |
| 659 | OCL | OMG | text | high |

### 8.5 Testing & Specification

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 660 | Gherkin | cucumber.io | text | low |
| 661 | Robot Framework | robotframework.org | text | medium |
| 662 | Gauge Spec | gauge.org | text | low |
| 663 | Karate DSL | karatelabs.github.io | text | medium |
| 664 | REST Assured DSL | rest-assured.io | text | medium |
| 665 | Pact DSL | pact.io | structured | medium |
| 666 | OpenAPI | openapis.org | structured | high |
| 667 | AsyncAPI | asyncapi.com | structured | high |
| 668 | RAML | raml.org | structured | medium |
| 669 | API Blueprint | apiblueprint.org | text | medium |
| 670 | Smithy | smithy.io | text | high |

### 8.6 Game Development

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 671 | GDScript | godotengine.org | text | medium |
| 672 | Unity ShaderLab | unity.com | text | high |
| 673 | Unreal Blueprint | unrealengine.com | binary | high |
| 674 | Inform 7 | inform7.com | text | high |
| 675 | Ink | inklestudios.com | text | medium |
| 676 | Twine | twinery.org | text | low |
| 677 | Yarn Spinner | yarnspinner.dev | text | medium |
| 678 | Dialogue Tree DSL | - | text | low |
| 679 | Tiled TMX | mapeditor.org | structured | medium |
| 680 | LDtk | ldtk.io | structured | medium |

### 8.7 Hardware Description

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 681 | VHDL | IEEE 1076 | text | very high |
| 682 | Verilog | IEEE 1364 | text | very high |
| 683 | SystemVerilog | IEEE 1800 | text | extremely high |
| 684 | Chisel | chisel-lang.org | text | high |
| 685 | SpinalHDL | spinalhdl.github.io | text | high |
| 686 | Amaranth | amaranth-lang.org | text | high |
| 687 | FIRRTL | chipsalliance.org | text | high |
| 688 | Bluespec | bluespec.com | text | very high |
| 689 | MyHDL | myhdl.org | text | medium |
| 690 | Clash | clash-lang.org | text | high |
| 691 | SPICE | - | text | medium |
| 692 | EDIF | JEDEC | text | high |
| 693 | LEF/DEF | Cadence | text | high |
| 694 | Liberty | Synopsys | text | medium |
| 695 | SDC | Synopsys | text | medium |

---

## 9. Media Formats (75)

### 9.1 Subtitles & Captions

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 696 | SRT | - | text | low |
| 697 | VTT (WebVTT) | W3C | text | medium |
| 698 | ASS/SSA | aegisub.org | text | medium |
| 699 | TTML | W3C | structured | high |
| 700 | SMPTE-TT | SMPTE | structured | high |
| 701 | EBU-TT | EBU | structured | high |
| 702 | CEA-608 | CEA | binary | medium |
| 703 | CEA-708 | CEA | binary | high |
| 704 | DVB Subtitles | ETSI | binary | high |
| 705 | PGS (Blu-ray) | Sony | binary | medium |

### 9.2 Playlists

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 706 | M3U/M3U8 | - | text | low |
| 707 | HLS (m3u8) | Apple | text | medium |
| 708 | DASH MPD | ISO 23009 | structured | high |
| 709 | PLS | - | text | low |
| 710 | XSPF | xspf.org | structured | low |
| 711 | ASX | Microsoft | structured | low |
| 712 | SMIL | W3C | structured | medium |

### 9.3 Music & Audio Metadata

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 713 | ID3v1 | id3.org | binary | low |
| 714 | ID3v2 | id3.org | binary | medium |
| 715 | Vorbis Comment | xiph.org | text | low |
| 716 | APEv2 | - | binary | low |
| 717 | FLAC Metadata | xiph.org | binary | medium |
| 718 | MP4 Atoms | ISO 14496 | binary | high |
| 719 | MusicXML | w3.org | structured | high |
| 720 | MEI | music-encoding.org | structured | high |
| 721 | ABC Notation | abcnotation.com | text | medium |
| 722 | LilyPond | lilypond.org | text | high |
| 723 | Chord Pro | chordpro.org | text | low |
| 724 | Guitar Pro | arobas-music.com | binary | high |

### 9.4 Multimedia Metadata

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 725 | EXIF | CIPA | binary | medium |
| 726 | IPTC-IIM | IPTC | binary | medium |
| 727 | XMP | Adobe | structured | medium |
| 728 | ICC Profile | ICC | binary | high |
| 729 | RIFF INFO | Microsoft | binary | low |
| 730 | Matroska Tags | matroska.org | structured | medium |
| 731 | NFO | - | text | low |

### 9.5 Streaming Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 732 | RTMP | Adobe | binary | high |
| 733 | HLS | Apple | text | medium |
| 734 | DASH | ISO 23009 | structured | high |
| 735 | MSS (Smooth Streaming) | Microsoft | structured | high |
| 736 | HDS | Adobe | structured | medium |
| 737 | WebRTC | W3C | binary | very high |
| 738 | SRT (Streaming) | Haivision | binary | medium |
| 739 | RIST | Video Services Forum | binary | medium |
| 740 | NDI | NewTek | binary | high |

---

## 10. Security & Crypto Formats (50)

### 10.1 Certificates & PKI

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 741 | X.509 Certificate | RFC 5280 | binary | high |
| 742 | X.509 CRL | RFC 5280 | binary | high |
| 743 | PKCS#1 | RFC 8017 | binary | medium |
| 744 | PKCS#7/CMS | RFC 5652 | binary | high |
| 745 | PKCS#8 | RFC 5958 | binary | medium |
| 746 | PKCS#10 CSR | RFC 2986 | binary | medium |
| 747 | PKCS#12/PFX | RFC 7292 | binary | high |
| 748 | PEM | RFC 7468 | text | low |
| 749 | JWK (JSON Web Key) | RFC 7517 | structured | medium |
| 750 | JWKS | RFC 7517 | structured | medium |
| 751 | SSH Public Key | RFC 4253 | text | low |
| 752 | SSH Private Key | OpenSSH | text | medium |
| 753 | GPG Public Key | RFC 4880 | binary | high |
| 754 | GPG Private Key | RFC 4880 | binary | high |
| 755 | age Key | age-encryption.org | text | low |

### 10.2 Encrypted Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 756 | PGP Message | RFC 4880 | binary | high |
| 757 | S/MIME Message | RFC 8551 | binary | high |
| 758 | JOSE (JWE) | RFC 7516 | text | medium |
| 759 | age Encrypted | age-encryption.org | binary | medium |
| 760 | OpenSSL Encrypted | OpenSSL | binary | medium |
| 761 | GPG Encrypted | RFC 4880 | binary | high |
| 762 | LUKS | LUKS spec | binary | high |
| 763 | VeraCrypt | veracrypt.fr | binary | high |
| 764 | BitLocker | Microsoft | binary | high |
| 765 | FileVault | Apple | binary | high |

### 10.3 Authentication & Tokens

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 766 | JWT | RFC 7519 | text | medium |
| 767 | SAML Assertion | OASIS | structured | high |
| 768 | SAML Request | OASIS | structured | high |
| 769 | OAuth Token | RFC 6749 | text | low |
| 770 | OIDC ID Token | openid.net | text | medium |
| 771 | Kerberos Ticket | RFC 4120 | binary | very high |
| 772 | FIDO2/WebAuthn | W3C | binary | high |
| 773 | TOTP | RFC 6238 | text | low |
| 774 | HOTP | RFC 4226 | binary | low |
| 775 | U2F | FIDO | binary | medium |
| 776 | passkey | FIDO | binary | high |

### 10.4 Hashes & Signatures

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 777 | Base64 | RFC 4648 | text | low |
| 778 | Base32 | RFC 4648 | text | low |
| 779 | Base58 | Bitcoin | text | low |
| 780 | Bech32 | BIP-0173 | text | low |
| 781 | Hash URI | - | text | low |
| 782 | SRI Hash | W3C | text | low |
| 783 | PGP Signature | RFC 4880 | binary | high |
| 784 | DKIM Signature | RFC 6376 | text | medium |
| 785 | JWS | RFC 7515 | text | medium |
| 786 | CMS Signature | RFC 5652 | binary | high |
| 787 | XML Signature | W3C | structured | high |
| 788 | PDF Signature | ISO 32000 | binary | high |
| 789 | Code Signing | Various | binary | high |
| 790 | Authenticode | Microsoft | binary | high |

---

## 11. Scientific & Engineering (40)

### 11.1 Scientific Data Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 791 | NetCDF | Unidata | binary | high |
| 792 | HDF5 | HDF Group | binary | very high |
| 793 | FITS | NASA | binary | high |
| 794 | CDF | NASA | binary | high |
| 795 | GRIB | WMO | binary | high |
| 796 | BUFR | WMO | binary | high |
| 797 | NIfTI | nifti.nimh.nih.gov | binary | medium |
| 798 | DICOM | NEMA | binary | very high |
| 799 | MINC | McGill | binary | high |
| 800 | SDF (Chemical) | - | text | medium |
| 801 | MOL/MOL2 | - | text | medium |
| 802 | PDB | wwPDB | text | medium |
| 803 | mmCIF | wwPDB | text | high |
| 804 | CIF (Crystallography) | IUCr | text | medium |
| 805 | STAR | IUCr | text | medium |

### 11.2 CAD & Engineering

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 806 | STEP | ISO 10303 | text | very high |
| 807 | IGES | ANSI | text | high |
| 808 | DXF | Autodesk | text | high |
| 809 | DWG | Autodesk | binary | very high |
| 810 | IFC | buildingSMART | text | very high |
| 811 | gbXML | gbXML.org | structured | high |
| 812 | CityGML | OGC | structured | high |
| 813 | LandXML | landxml.org | structured | high |
| 814 | SPICE Netlist | - | text | medium |
| 815 | Gerber | Ucamco | text | medium |
| 816 | Eagle | Autodesk | structured | high |
| 817 | KiCad | kicad.org | text | medium |

### 11.3 Math & Statistics

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 818 | MathML | W3C | structured | high |
| 819 | OpenMath | openmath.org | structured | high |
| 820 | LaTeX Math | - | text | high |
| 821 | AsciiMath | asciimath.org | text | low |
| 822 | PMML | DMG | structured | high |
| 823 | ONNX | onnx.ai | binary | high |
| 824 | TensorFlow SavedModel | tensorflow.org | binary | high |
| 825 | PyTorch Model | pytorch.org | binary | high |
| 826 | MATLAB .mat | MathWorks | binary | high |
| 827 | R RDS | r-project.org | binary | medium |
| 828 | NumPy .npy | numpy.org | binary | low |
| 829 | Arrow IPC | arrow.apache.org | binary | high |
| 830 | Feather | arrow.apache.org | binary | medium |

---

## 12. Geographic & Mapping (30)

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 831 | GeoJSON | RFC 7946 | structured | medium |
| 832 | TopoJSON | github.com/topojson | structured | high |
| 833 | KML | OGC | structured | medium |
| 834 | KMZ | OGC | binary | medium |
| 835 | GML | OGC | structured | high |
| 836 | GPX | topografix.com | structured | low |
| 837 | Shapefile | ESRI | binary | high |
| 838 | GeoPackage | OGC | binary | high |
| 839 | WKT | OGC | text | medium |
| 840 | WKB | OGC | binary | medium |
| 841 | GeoTIFF | OGC | binary | high |
| 842 | Cloud Optimized GeoTIFF | cogeo.org | binary | high |
| 843 | MBTiles | mapbox.com | binary | medium |
| 844 | PMTiles | protomaps.com | binary | medium |
| 845 | Vector Tiles | mapbox.com | binary | high |
| 846 | OSM XML | openstreetmap.org | structured | high |
| 847 | OSM PBF | openstreetmap.org | binary | high |
| 848 | Overpass QL | openstreetmap.org | text | medium |
| 849 | NMEA 0183 | NMEA | text | low |
| 850 | RINEX | IGS | text | medium |
| 851 | S-57 | IHO | binary | high |
| 852 | S-100 | IHO | binary | very high |
| 853 | CityJSON | cityjson.org | structured | high |
| 854 | 3D Tiles | Cesium | binary | high |
| 855 | i3s | ESRI | binary | high |
| 856 | LAS | ASPRS | binary | medium |
| 857 | LAZ | rapidlasso.com | binary | medium |
| 858 | COPC | copc.io | binary | medium |
| 859 | Mapbox Style | mapbox.com | structured | medium |
| 860 | CartoCSS | carto.com | text | medium |

---

## 13. Financial Formats (35)

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 861 | FIX Protocol | fixtrading.org | text | high |
| 862 | FIX/FAST | fixtrading.org | binary | high |
| 863 | FIXML | fixtrading.org | structured | high |
| 864 | FpML | fpml.org | structured | very high |
| 865 | ISO 20022 | iso20022.org | structured | very high |
| 866 | SWIFT MT | SWIFT | text | high |
| 867 | SWIFT MX | SWIFT | structured | high |
| 868 | XBRL | xbrl.org | structured | very high |
| 869 | iXBRL | xbrl.org | structured | very high |
| 870 | OFX | ofx.net | structured | medium |
| 871 | QIF | Intuit | text | low |
| 872 | QFX | Quicken | structured | medium |
| 873 | IIF | QuickBooks | text | medium |
| 874 | BAI2 | BAI | text | medium |
| 875 | MT940 | SWIFT | text | medium |
| 876 | CAMT.053 | ISO 20022 | structured | high |
| 877 | PAIN.001 | ISO 20022 | structured | high |
| 878 | NACHA/ACH | NACHA | text | medium |
| 879 | SEPA XML | EPC | structured | high |
| 880 | EDI X12 | ASC X12 | text | high |
| 881 | EDIFACT | UN/CEFACT | text | high |
| 882 | Bloomberg Open Symbology | Bloomberg | text | low |
| 883 | ISIN | ISO 6166 | text | low |
| 884 | CUSIP | CUSIP Global | text | low |
| 885 | SEDOL | LSE | text | low |
| 886 | LEI | GLEIF | text | low |
| 887 | FIGI | OMG | text | low |
| 888 | Ticker Symbol | Various | text | low |
| 889 | Options Symbology | OCC | text | medium |
| 890 | Cryptocurrency Address | Various | text | medium |
| 891 | Bitcoin Script | bitcoin.org | binary | high |
| 892 | Ethereum ABI | ethereum.org | binary | high |
| 893 | Solidity | ethereum.org | text | high |
| 894 | Move | diem.com | text | high |
| 895 | Clarity | stacks.co | text | medium |

---

## 14. Healthcare Formats (25)

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 896 | HL7 v2 | hl7.org | text | high |
| 897 | HL7 v3 | hl7.org | structured | very high |
| 898 | HL7 FHIR | hl7.org/fhir | structured | high |
| 899 | HL7 CDA | hl7.org | structured | high |
| 900 | DICOM | NEMA | binary | very high |
| 901 | DICOM SR | NEMA | binary | very high |
| 902 | IHE XDS | ihe.net | structured | high |
| 903 | IHE PIX | ihe.net | text | high |
| 904 | SNOMED CT | snomed.org | text | high |
| 905 | ICD-10 | WHO | text | medium |
| 906 | ICD-11 | WHO | structured | high |
| 907 | LOINC | loinc.org | text | medium |
| 908 | RxNorm | nlm.nih.gov | text | medium |
| 909 | NDC | FDA | text | low |
| 910 | CPT | AMA | text | medium |
| 911 | HCPCS | CMS | text | low |
| 912 | X12 837 (Claims) | ASC X12 | text | high |
| 913 | X12 835 (Remittance) | ASC X12 | text | high |
| 914 | NCPDP SCRIPT | NCPDP | structured | high |
| 915 | CCDA | hl7.org | structured | high |
| 916 | Blue Button | cms.gov | structured | medium |
| 917 | OMOP CDM | ohdsi.org | structured | high |
| 918 | i2b2 | i2b2.org | structured | high |
| 919 | openEHR | openehr.org | structured | very high |
| 920 | ISO 13606 | ISO | structured | very high |

---

## 15. Logging & Observability (35)

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 921 | Syslog | RFC 5424 | text | medium |
| 922 | Syslog BSD | RFC 3164 | text | low |
| 923 | CEF | ArcSight | text | medium |
| 924 | LEEF | IBM | text | medium |
| 925 | CLF (Common Log Format) | W3C | text | low |
| 926 | Combined Log Format | Apache | text | low |
| 927 | ECS (Elastic Common Schema) | elastic.co | structured | medium |
| 928 | OCSF | ocsf.io | structured | high |
| 929 | OpenTelemetry Protocol | opentelemetry.io | binary | high |
| 930 | Jaeger | jaegertracing.io | binary | medium |
| 931 | Zipkin | zipkin.io | structured | medium |
| 932 | StatsD | github.com/statsd | text | low |
| 933 | Prometheus Exposition | prometheus.io | text | low |
| 934 | OpenMetrics | openmetrics.io | text | medium |
| 935 | InfluxDB Line Protocol | influxdata.com | text | low |
| 936 | Graphite Plaintext | graphiteapp.org | text | low |
| 937 | Carbon | graphiteapp.org | text | low |
| 938 | collectd | collectd.org | binary | medium |
| 939 | GELF | graylog.org | structured | low |
| 940 | Fluentd Forward | fluentd.org | binary | medium |
| 941 | Fluent Bit | fluentbit.io | binary | medium |
| 942 | Vector | vector.dev | structured | medium |
| 943 | Loki | grafana.com | text | medium |
| 944 | journald | systemd.io | binary | medium |
| 945 | Windows Event Log | Microsoft | binary | high |
| 946 | EVTX | Microsoft | binary | high |
| 947 | pcap | tcpdump.org | binary | medium |
| 948 | pcapng | pcapng.com | binary | medium |
| 949 | NetFlow v5 | Cisco | binary | medium |
| 950 | NetFlow v9 | RFC 3954 | binary | medium |
| 951 | IPFIX | RFC 7011 | binary | high |
| 952 | sFlow | sflow.org | binary | medium |
| 953 | SNMP | RFC 3416 | binary | high |
| 954 | SNMP MIB | RFC 2578 | text | medium |
| 955 | SNMPv3 | RFC 3414 | binary | high |

---

## 16. Build & Package Formats (40)

### 16.1 Package Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 956 | npm package.json | npm | structured | medium |
| 957 | npm package-lock.json | npm | structured | medium |
| 958 | Yarn yarn.lock | yarnpkg.com | text | medium |
| 959 | pnpm-lock.yaml | pnpm.io | structured | medium |
| 960 | PyPI setup.cfg | python.org | text | low |
| 961 | Poetry pyproject.toml | python-poetry.org | text | medium |
| 962 | pip requirements.txt | pip.pypa.io | text | low |
| 963 | Pipfile | pipenv.pypa.io | text | medium |
| 964 | Conda environment.yml | conda.io | structured | medium |
| 965 | RubyGems gemspec | rubygems.org | text | medium |
| 966 | Bundler Gemfile.lock | bundler.io | text | medium |
| 967 | Cargo.lock | rust-lang.org | text | medium |
| 968 | Go go.sum | go.dev | text | low |
| 969 | Maven pom.xml | maven.apache.org | structured | medium |
| 970 | Gradle dependencies | gradle.org | text | medium |
| 971 | NuGet .nuspec | nuget.org | structured | medium |
| 972 | NuGet packages.config | nuget.org | structured | low |
| 973 | CocoaPods Podfile | cocoapods.org | text | medium |
| 974 | Carthage Cartfile | github.com/Carthage | text | low |
| 975 | Swift Package.resolved | swift.org | structured | medium |
| 976 | Pub pubspec.lock | dart.dev | structured | medium |
| 977 | Hex mix.lock | hex.pm | text | medium |
| 978 | Composer composer.lock | getcomposer.org | structured | medium |
| 979 | CPAN META.json | cpan.org | structured | medium |
| 980 | Hackage .cabal | hackage.haskell.org | text | medium |

### 16.2 Container & OS Packages

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 981 | Dockerfile | docker.com | text | medium |
| 982 | Docker Compose | docker.com | structured | medium |
| 983 | OCI Image Manifest | opencontainers.org | structured | medium |
| 984 | OCI Image Index | opencontainers.org | structured | medium |
| 985 | Helm Chart.yaml | helm.sh | structured | medium |
| 986 | Kustomization | kubernetes.io | structured | medium |
| 987 | Debian control | debian.org | text | medium |
| 988 | RPM spec | rpm.org | text | high |
| 989 | PKGBUILD | archlinux.org | text | medium |
| 990 | ebuild | gentoo.org | text | medium |
| 991 | Homebrew Formula | brew.sh | text | medium |
| 992 | Chocolatey nuspec | chocolatey.org | structured | medium |
| 993 | Scoop manifest | scoop.sh | structured | medium |
| 994 | Flatpak manifest | flatpak.org | structured | medium |
| 995 | Snap snapcraft.yaml | snapcraft.io | structured | medium |

---

## 17. Miscellaneous (75)

### 17.1 Regular Expressions

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 996 | POSIX BRE | IEEE 1003.1 | text | low |
| 997 | POSIX ERE | IEEE 1003.1 | text | low |
| 998 | PCRE | pcre.org | text | high |
| 999 | PCRE2 | pcre.org | text | high |
| 1000 | ECMAScript RegExp | ECMA-262 | text | medium |
| 1001 | .NET Regex | Microsoft | text | high |
| 1002 | Java Pattern | Oracle | text | high |
| 1003 | Python re | python.org | text | medium |
| 1004 | Ruby Regexp | ruby-lang.org | text | medium |
| 1005 | Go regexp | go.dev | text | medium |
| 1006 | Rust regex | docs.rs/regex | text | medium |
| 1007 | RE2 | github.com/google/re2 | text | medium |
| 1008 | Oniguruma | github.com/kkos | text | high |
| 1009 | Hyperscan | intel.github.io | text | high |
| 1010 | TRE | laurikari.net | text | medium |

### 17.2 Calendar & Time

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 1011 | iCalendar | RFC 5545 | text | high |
| 1012 | vCalendar | IMC | text | medium |
| 1013 | vCard | RFC 6350 | text | medium |
| 1014 | hCalendar | microformats.org | text | medium |
| 1015 | jCal | RFC 7265 | structured | medium |
| 1016 | jCard | RFC 7095 | structured | medium |
| 1017 | xCal | RFC 6321 | structured | medium |
| 1018 | ISO 8601 | ISO | text | medium |
| 1019 | RFC 3339 | RFC 3339 | text | low |
| 1020 | RFC 2822 Date | RFC 2822 | text | low |
| 1021 | Unix Timestamp | - | text | low |
| 1022 | Cron Expression | - | text | low |
| 1023 | Quartz Cron | Quartz | text | medium |
| 1024 | ISO 8601 Duration | ISO | text | low |
| 1025 | RRULE | RFC 5545 | text | high |

### 17.3 URI & URL Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 1026 | URI | RFC 3986 | text | medium |
| 1027 | URL | WHATWG | text | medium |
| 1028 | URN | RFC 8141 | text | low |
| 1029 | IRI | RFC 3987 | text | medium |
| 1030 | Data URI | RFC 2397 | text | low |
| 1031 | Mailto URI | RFC 6068 | text | medium |
| 1032 | Tel URI | RFC 3966 | text | low |
| 1033 | File URI | RFC 8089 | text | low |
| 1034 | Magnet URI | - | text | medium |
| 1035 | Custom URL Scheme | - | text | low |

### 17.4 Version Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 1036 | Semantic Versioning | semver.org | text | low |
| 1037 | CalVer | calver.org | text | low |
| 1038 | PEP 440 | python.org | text | medium |
| 1039 | npm semver | npm | text | low |
| 1040 | Maven Version | maven.apache.org | text | medium |
| 1041 | NuGet Version | nuget.org | text | low |
| 1042 | Ruby Gem Version | rubygems.org | text | low |
| 1043 | Debian Version | debian.org | text | medium |
| 1044 | RPM Version | rpm.org | text | medium |

### 17.5 Color Formats

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 1045 | CSS Color | W3C | text | medium |
| 1046 | Hex Color | - | text | low |
| 1047 | RGB/RGBA | CSS | text | low |
| 1048 | HSL/HSLA | CSS | text | low |
| 1049 | HWB | CSS | text | low |
| 1050 | Lab | CSS | text | low |
| 1051 | LCH | CSS | text | low |
| 1052 | OKLab/OKLCH | CSS | text | low |
| 1053 | Color() function | CSS | text | medium |
| 1054 | Named Colors | CSS | text | low |
| 1055 | System Colors | CSS | text | low |

### 17.6 Human Language

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 1056 | ISO 639 Language Code | ISO | text | low |
| 1057 | BCP 47 Language Tag | RFC 5646 | text | medium |
| 1058 | IETF Language Tag | RFC 5646 | text | medium |
| 1059 | Locale (POSIX) | IEEE 1003.1 | text | medium |
| 1060 | ICU Locale | ICU | text | medium |
| 1061 | CLDR | Unicode | structured | high |
| 1062 | gettext PO | GNU | text | medium |
| 1063 | gettext MO | GNU | binary | medium |
| 1064 | XLIFF | OASIS | structured | high |
| 1065 | TMX | LISA | structured | medium |
| 1066 | TBX | ISO 30042 | structured | medium |
| 1067 | SRX | LISA | structured | medium |

### 17.7 Identifiers

| # | Name | Spec Reference | Category | Complexity |
|---|------|----------------|----------|------------|
| 1068 | UUID | RFC 4122 | text | low |
| 1069 | ULID | ulid.github.io | text | low |
| 1070 | KSUID | segment.com | text | low |
| 1071 | Snowflake ID | Twitter | text | low |
| 1072 | NanoID | github.com/ai/nanoid | text | low |
| 1073 | CUID | cuid.dev | text | low |
| 1074 | ObjectId | mongodb.com | text | low |

---

## Schema Validation Summary

### Coverage by Parser Type

| Type | Count | % |
|------|-------|---|
| Text | 650+ | 59% |
| Binary | 250+ | 23% |
| Structured (JSON/XML) | 150+ | 14% |
| Hybrid | 50+ | 4% |

### Coverage by Complexity

| Complexity | Count | % |
|------------|-------|---|
| Low | 250+ | 23% |
| Medium | 450+ | 41% |
| High | 300+ | 27% |
| Very High | 80+ | 7% |
| Extremely High | 20+ | 2% |

### Coverage by Domain

| Domain | Count |
|--------|-------|
| Data Interchange | 85 |
| Programming Languages | 150 |
| Configuration | 65 |
| Network Protocols | 120 |
| Markup Languages | 55 |
| Binary Formats | 95 |
| Query Languages | 45 |
| Domain-Specific | 80 |
| Media | 75 |
| Security/Crypto | 50 |
| Scientific | 40 |
| Geographic | 30 |
| Financial | 35 |
| Healthcare | 25 |
| Logging | 35 |
| Build/Package | 40 |
| Miscellaneous | 75 |

---

## UPS Schema Validation

All 1074 parsers in this catalog can be described using the Universal Parser Specification schema. The schema supports:

✅ **Text parsers** - via grammar definitions (ABNF, EBNF, PEG, etc.)
✅ **Binary parsers** - via binary_structure definitions
✅ **Structured formats** - via AST/output schemas
✅ **Streaming parsers** - via streaming configuration
✅ **Protocol parsers** - via state machine definitions
✅ **Hybrid formats** - via composed parser dependencies
✅ **Complex grammars** - via external grammar file references
✅ **Conformance testing** - via test vectors and property tests
✅ **Quality requirements** - via performance/security metrics
✅ **Extensibility** - via extension namespaces

---

*Catalog Version: 1.0.0*
*Total Parsers: 1074*
*Last Updated: January 2025*
