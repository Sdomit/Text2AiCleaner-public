<div align="center">

# 🧹 Text2AiCleaner

**Sanitize logs, paths, and private data before sharing with AI.**

[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![Phase 1 Complete](https://img.shields.io/badge/Phase%201-Complete-brightgreen?style=flat-square)]()
[![Tests: 86 passing](https://img.shields.io/badge/Tests-86%20passing-brightgreen?style=flat-square)]()
[![100% Local](https://img.shields.io/badge/100%25-Local%20%26%20Offline-blue?style=flat-square)]()

A local-first privacy tool that detects and replaces sensitive information in text  
with consistent labeled placeholders — before you paste anything into ChatGPT, Claude,  
Cursor, Gemini, or Copilot.

</div>

---

## 🔍 The Problem

Every day, developers and technical users paste things like this into AI tools:

- 🖥️ Server logs with real IP addresses and machine names
- 📂 Stack traces with internal paths and usernames
- 🔑 Config files with credentials and hostnames
- 🎬 Render farm logs with studio infrastructure details
- 🎫 Support tickets with client names and project paths

Current tools are cloud-based, destructive, or enterprise-only.  
Text2AiCleaner is none of those things.

---

## ⚙️ How It Works

**Input:**
```
\10.0.25.10\DeadlineRepo\Greywalds_Chronicles\shots\101\101_010
Worker: render-node-045
User: sarmad.domit
Email: sarmad.domit@studio.internal
```

**Output:**
```
\SERVER_001\PATH_001
Worker: MACHINE_001
User: USER_001
Email: EMAIL_001
```

✅ Structure preserved. ✅ Meaning preserved. ✅ Privacy protected.

---

## 🛡️ What Gets Detected

| Type | Examples | Placeholder |
|:---|:---|:---|
| 🌐 IPv4 addresses | `192.168.1.1`, `10.0.25.10` | `IP_001` |
| 📧 Email addresses | `user@corp.local` | `EMAIL_001` |
| 🗂️ Windows paths | `C:\Users\john\logs` | `PATH_001` |
| 🔗 UNC network paths | `\server01\share\path` | `PATH_001` |
| 🐧 Linux / POSIX paths | `/home/alice/config.yaml` | `PATH_001` |
| 🌍 URLs | `https://internal.corp.local/api` | `DOMAIN_001` |
| 🏷️ Domains | `corp.local`, `build.internal` | `DOMAIN_001` |
| 💻 Hostnames / machines | `render-node-045`, `DESKTOP-ABC123` | `MACHINE_001` |
| 👤 Usernames (from paths) | extracted from `C:\Users\john` | `USER_001` |

Each unique value gets a consistent placeholder across the entire file.

---

## 💻 CLI

```bash
# Clean a file
safetext clean mylog.txt

# Preview detections without writing any files
safetext clean mylog.txt --dry-run

# Show a colored side-by-side diff
safetext clean mylog.txt --diff

# Clean inline text
safetext clean --text "connect to 192.168.1.1 as john@corp.local"

# Read from stdin
cat mylog.txt | safetext clean -
```

**Dry-run preview:**
```
safetext clean sample_input.txt --dry-run

Found 7 sensitive values:
  [MACHINE]   render-node-045         (1 occurrence)  ->  MACHINE_001
  [IP]        10.0.99.1               (1 occurrence)  ->  IP_001
  [PATH]      \render-farm-01\...    (1 occurrence)  ->  PATH_001
  [PATH]      C:\Users\testuser\...   (1 occurrence)  ->  PATH_002
  [EMAIL]     testuser@studio.int..   (1 occurrence)  ->  EMAIL_001
  [DOMAIN]    https://pipeline...     (1 occurrence)  ->  DOMAIN_001
  [MACHINE]   build-server-dev-01     (1 occurrence)  ->  MACHINE_002
```

---

## 📁 Output Files

For input `mylog.txt`, four files are written:

| File | Contents | Safe to share? |
|:---|:---|:---:|
| `mylog_SAFE.txt` | Cleaned text | ✅ Yes |
| `mylog_REPORT.json` | What was found and replaced | ❌ No — contains raw values |
| `mylog_REPORT.csv` | Same in CSV format | ❌ No — contains raw values |
| `mylog_MAPPING.json` | `raw value → placeholder` map | ❌ No — keep private |

---

## 🗺️ Roadmap

| Phase | Status | What's in it |
|:---|:---:|:---|
| **1 — Regex MVP** | ✅ Done | Detection, CLI, reports, 86 tests |
| **2 — Professional Tooling** | 🔧 In Progress | Batch mode, preset profiles, cross-run mapping, `.exe` |
| **3 — NLP Integration** | 📋 Planned | spaCy + GLiNER named entity recognition |
| **4 — Local AI** | 📋 Planned | Ollama / Qwen semantic detection, no cloud |
| **5 — Enterprise & Datasets** | 📋 Planned | Dataset anonymization, plugin ecosystem, policy systems |

---

## 🏭 Target Industries

| Industry | Common Use Cases |
|:---|:---|
| 🎬 VFX & Animation | render logs, project paths, review data |
| 🖥️ IT & Sysadmin | infrastructure logs, configs, incident reports |
| 💻 Software Development | stack traces, internal repositories |
| 🤖 AI & ML | dataset anonymization for training data |
| 🎧 Support Teams | safe bug reports with sanitized attachments |
| 🔬 Research | publishable sanitized datasets |

---

## 🔒 Source Code

The full source code is in a private repository.  
This public repo contains documentation, demos, and the roadmap only.

---

## 📄 License

MIT
