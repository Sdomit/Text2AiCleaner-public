# 🧹 Text2AiCleaner

**Sanitize logs, paths, and private data before sharing with AI.**

![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)
![Phase 1 Complete](https://img.shields.io/badge/Phase%201-Complete-brightgreen?style=flat-square)
![Tests: 86 passing](https://img.shields.io/badge/Tests-86%20passing-brightgreen?style=flat-square)
![100% Local](https://img.shields.io/badge/100%25-Local%20%26%20Offline-blue?style=flat-square)

A local-first privacy tool that detects and replaces sensitive information in text with consistent labeled placeholders — before you paste anything into ChatGPT, Claude, Cursor, Gemini, or Copilot.

> Text2AiCleaner also serves as the privacy/anonymization engine for the
> [VFX Pipeline Intelligence](https://github.com/Sdomit/vfx-pipeline-intelligence-showcase)
> project — turning identifiers into safe, consistent codes that double as ML features.

---

## 🔍 The Problem

Every day, technical users paste sensitive text into AI tools:

- 🖥️ Server logs with real IPs and machine names
- 📂 Stack traces with internal paths and usernames
- 🔑 Config files with credentials and hostnames
- 🎬 Render farm logs with studio infrastructure details
- 🎫 Support tickets with client names and project paths

Text2AiCleaner replaces all of that with consistent placeholders, locally, before anything leaves your machine.

---

## ⚙️ How It Works

**Input:**

```
\\10.0.25.10\DeadlineRepo\Project_X\shots\101\101_010
Worker: render-node-045
User: sarmad.domit
Email: sarmad.domit@studio.internal
```

**Output:**

```
\\SERVER_001\PATH_001
Worker: MACHINE_001
User: USER_001
Email: EMAIL_001
```

✅ Structure preserved ✅ Meaning preserved ✅ Privacy protected

Each unique value gets a consistent placeholder across the whole file, so the text stays coherent for the AI while the real values never leave your machine.

---

## 🛡️ What Gets Detected

| Type | Example | Placeholder |
| --- | --- | --- |
| 🌐 IPv4 | `10.0.25.10` | `IP_001` |
| 📧 Email | `user@corp.local` | `EMAIL_001` |
| 🗂️ Windows path | `C:\Users\john\logs` | `PATH_001` |
| 🔗 UNC path | `\\server01\share` | `PATH_001` |
| 🐧 POSIX path | `/home/alice/config.yaml` | `PATH_001` |
| 🌍 URL / domain | `internal.corp.local` | `DOMAIN_001` |
| 💻 Hostname | `render-node-045` | `MACHINE_001` |
| 👤 Username (from paths) | from `C:\Users\john` | `USER_001` |

---

## 💻 CLI

```
# Clean a file
safetext clean mylog.txt

# Preview detections without writing files
safetext clean mylog.txt --dry-run

# Colored side-by-side diff
safetext clean mylog.txt --diff

# Clean inline text
safetext clean --text "connect to 10.0.25.10 as john@corp.local"

# Read from stdin
cat mylog.txt | safetext clean -
```

---

## 📁 Output Files

For input `mylog.txt`, four files are written:

| File | Contents | Safe to share? |
| --- | --- | --- |
| `mylog_SAFE.txt` | Cleaned text | ✅ Yes |
| `mylog_REPORT.json` | What was found and replaced | ❌ No — raw values |
| `mylog_REPORT.csv` | Same, CSV | ❌ No — raw values |
| `mylog_MAPPING.json` | `raw → placeholder` map | ❌ No — keep private |

> ⚠️ The `_MAPPING.json` file contains the original sensitive values. It is the one file you must never commit or share. Encrypted mapping is on the roadmap.

---

## 🗺️ Roadmap

> **Phases 1–2 are the committed, working scope.** Phases 3+ are exploratory directions, listed for transparency — not active promises.

| Phase | Status | What's in it |
| --- | --- | --- |
| **1 — Regex MVP** | ✅ Done | Detection, CLI, reports, 86 tests |
| **2 — Professional tooling** | 🔧 In progress | Batch mode, preset profiles, cross-run mapping, encrypted mapping, `.exe` |
| **3 — NLP (exploratory)** | 💡 Maybe | spaCy / GLiNER named-entity recognition |
| **4 — Local AI (exploratory)** | 💡 Maybe | Optional local model for semantic detection, fully offline |

---

## 🏭 Where It's Useful

| Domain | Use cases |
| --- | --- |
| 🎬 VFX & animation | render logs, project paths, review data |
| 🖥️ IT & sysadmin | infrastructure logs, configs, incident reports |
| 💻 Software dev | stack traces, internal repos |
| 🎧 Support teams | safe bug reports with sanitized attachments |

---

## 🔒 Source Code

The full source lives in a private repository. This public repo contains documentation, demos, and the roadmap.

---

## 📄 License

MIT
