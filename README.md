# REVKIT — Universal EXE Reverse Engineering Toolkit

**REVKIT** is an automated, lightweight reverse engineering toolkit designed for Windows PE binaries (`.exe` and `.dll`). It identifies the source programming language, runs specialized extraction and disassembly pipelines, and compiles structured findings into a consolidated `REPORT.md`.

---

## Key Features

- **Automated PE Inspection**:
  - Parses DOS & NT headers, sections, entry point, image base, and machine architecture.
  - Computes section Shannon entropy to detect packed or encrypted payloads.
  - Safely parses Import (IAT), Export (EAT), and Debug (PDB metadata) tables.
- **Language Heuristics**:
  - Multi-indicator scoring engine for **Go**, **Python** (PyInstaller, PyArmor, Nuitka), **Rust**, **.NET (CLR)**, **Delphi**, **AutoIt**, and **Native C/C++**.
- **Specialized Extraction Pipelines**:
  - **Go**: Extracts Go build ID, Go version, module path, dependencies, and source paths. Disassembles functions using `go tool objdump` and integrates with `GoReSym`.
  - **Python**: Detects PyInstaller archives, extracts `.pyc` payloads, and runs decompilation tools (`decompyle3`, `uncompyle6`, `pycdc`).
  - **Rust**: Discovers mangled symbol signatures and runs symbol demangling via `rustfilt`.
  - **.NET**: Inspects CLR directories and invokes `ilspycmd` for automated C# decompilation.
  - **Native C/C++**: Fallback heuristics for strings (API endpoints, credentials, URLs, error messages).
- **Consolidated AI-Ready Reports**:
  - Outputs a complete markdown dossier (`REPORT.md`) containing all findings, ready to feed into AI assistants or analysts.

---

## Requirements

- Python 3.8 or newer
- Dependencies:
  ```bash
  pip install -r requirements.txt
  ```

### Optional External Tools
- **Go**: Official Go toolchain (`go`) and [GoReSym](https://github.com/mandiant/GoReSym)
- **Python**: `pyinstxtractor` / `pyinstxtractor-ng`, `uncompyle6`, `decompyle3`, or `pycdc`
- **Rust**: `cargo install rustfilt`
- **.NET**: `dotnet tool install -g ilspycmd`

---

## Usage

### 1. Interactive Mode (Drag & Drop)
Run the script without arguments. A prompt will guide you to drag and drop your binary:
```bash
python reverse.py
```

### 2. Direct CLI Analysis
Analyze a specific executable directly:
```bash
python reverse.py path/to/target.exe
```

### 3. Deep Analysis Mode (Go Binaries)
Disassemble all `main.*` functions and recover string literals from assembly references:
```bash
python reverse.py path/to/target.exe --deep
```

---

## Output Structure

All outputs are saved to `<target_name>_reversed/`:
```
<target_name>_reversed/
├── REPORT.md             # Complete analysis summary dossier
├── sections.txt          # Section addresses, sizes, entropy, and flags
├── imports.txt           # Imported DLLs and functions
├── strings_ascii.txt     # Extracted ASCII strings (>= 5 chars)
├── strings_utf16.txt     # Extracted UTF-16 LE strings (>= 5 chars)
├── go_build_info.txt     # Go module & dependency data (Go binaries)
├── go_symbols.txt        # Extracted symbols (Go binaries)
├── go_asm_strings.txt    # Recovered string literals from asm (Go binaries)
└── disasm/               # Disassembly listings for main functions
```

---

## License & Disclaimer

This project is intended for educational purposes, security analysis, malware investigation, and software interoperability research. Only analyze binaries you are authorized to inspect.
