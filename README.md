# Text2AiCleaner

> **Sanitize logs, paths, and private data before sharing with AI.**

A local-first privacy tool that detects and replaces sensitive information in text
with consistent labeled placeholders — before you paste anything into ChatGPT, Claude,
Cursor, Gemini, or Copilot.

**100% local. No cloud. No data leaves your machine.**

---

## The Problem

Every day, developers and technical users share:

- Server logs with real IP addresses
- Stack traces with internal paths and usernames
- Config files with machine names and credentials
- Render logs with studio infrastructure details
- Support tickets with client names and project paths

Current tools are cloud-based, destructive, or enterprise-only.

---

## How It Works

**Input:**
```
\\10.0.25.10\DeadlineRepo\Greywalds_Chronicles\shots\101\101_010
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

Structure preserved. Meaning preserved. Privacy protected.

---

## What Gets Detected (Phase 1)

| Type | Examples | Placeholder |
|---|---|---|
| IPv4 addresses | `192.168.1.1`, `10.0.25.10` | `IP_001` |
| Email addresses | `user@corp.local` | `EMAIL_001` |
| Windows paths | `C:\Users\john\logs` | `PATH_001` |
| UNC network paths | `\\server01\share\path` | `PATH_001` |
| Linux/POSIX paths | `/home/alice/config.yaml` | `PATH_001` |
| URLs | `https://internal.corp.local/api` | `DOMAIN_001` |
| Domains | `corp.local`, `build.internal` | `DOMAIN_001` |
| Hostnames/machines | `render-node-045`, `DESKTOP-ABC123` | `MACHINE_001` |
| Usernames (from paths) | extracted from `C:\Users\john` | `USER_001` |

---

## Demo

### Input (`sample_input.txt`)

```
Render job failed on worker render-node-045
Worker address: 10.0.99.1
User: testuser
UNC path: \\render-farm-01\DeadlineRepo\SampleProject\shots\101\101_010
Windows path: C:\Users\testuser\Documents\render_logs\frame_0042.log
Linux log: /var/log/deadline/render_0042.log
Email: testuser@studio.internal
URL: https://pipeline.studio.internal/api/v1/jobs/0042
Machine: build-server-dev-01
```

### Output (`sample_input_SAFE.txt`)

```
Render job failed on worker MACHINE_001
Worker address: IP_001
User: USER_001
UNC path: PATH_001
Windows path: PATH_002
Linux log: PATH_003
Email: EMAIL_001
URL: DOMAIN_001
Machine: MACHINE_002
```

### Dry-run preview

```
safetext clean sample_input.txt --dry-run

Found 12 sensitive values:
  [MACHINE]   render-node-045         (1 occurrence)  -> MACHINE_001
  [IP]        10.0.99.1               (1 occurrence)  -> IP_001
  [PATH]      \\render-farm-01\...    (1 occurrence)  -> PATH_001
  [PATH]      C:\Users\testuser\...   (1 occurrence)  -> PATH_002
  [EMAIL]     testuser@studio.int..   (1 occurrence)  -> EMAIL_001
  [DOMAIN]    https://pipeline...     (1 occurrence)  -> DOMAIN_001
  [MACHINE]   build-server-dev-01     (1 occurrence)  -> MACHINE_002
```

---

## CLI Usage

```bash
# Clean a file
safetext clean mylog.txt

# Preview detections without writing files
safetext clean mylog.txt --dry-run

# Show a colored diff
safetext clean mylog.txt --diff

# Clean inline text
safetext clean --text "connect to 192.168.1.1 as john@corp.local"

# Read from stdin
cat mylog.txt | safetext clean -

# Clean an entire folder (Phase 2)
safetext clean logs/ --output-dir cleaned_logs/
```

---

## Output Files

For input `mylog.txt`:

| File | Contents | Safe to share? |
|---|---|---|
| `mylog_SAFE.txt` | Cleaned text | **Yes** |
| `mylog_REPORT.json` | What was found and replaced | No (contains raw values) |
| `mylog_REPORT.csv` | Same in CSV format | No |
| `mylog_MAPPING.json` | `raw → placeholder` map | No |

---

## Screenshots

_Coming soon — see the `screenshots/` folder._

---

## Roadmap

### Phase 1 — MVP ✅ Complete
- Regex-based detection (IP, email, paths, URLs, domains, hostnames)
- Consistent placeholder mapping within one run
- `--dry-run`, `--diff`, `--text`, stdin support
- JSON + CSV + mapping report output
- YAML config for custom patterns
- 86 tests

### Phase 2 — Professional Tooling 🔧 In Progress
- Batch mode: clean entire folders recursively
- Profile system: VFX, IT, Developer, Medical presets
- Cross-run mapping consistency
- Windows `.exe` packaging (PyInstaller)

### Phase 3 — NLP Integration
- spaCy named entity recognition
- GLiNER entity detection
- Catches names, organizations, free-form PII

### Phase 4 — Local AI Integration
- Ollama + Qwen2.5/Qwen3 semantic detection
- Context-aware sensitivity scoring
- No cloud required

### Phase 5 — Enterprise & Dataset Features
- Dataset anonymization for ML pipelines
- Plugin ecosystem
- Enterprise policy systems

---

## Target Industries

| Industry | Use Cases |
|---|---|
| VFX & Animation | render logs, project paths, review data |
| IT & Sysadmin | infrastructure logs, configs |
| Software Development | stack traces, repositories |
| AI & ML | dataset anonymization |
| Support Teams | safe bug reporting |
| Research | publishable sanitized datasets |

---

## Source Code

The full source code is in a private repository.
This public repo contains documentation, demos, and the roadmap only.

---

## License

MIT
