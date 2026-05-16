# Text2AiCleaner — Roadmap

## Phase 1 — MVP ✅ Complete

**Regex-based local sanitization CLI**

- IPv4, email, Windows/UNC/Linux paths, URLs, domains, hostnames, usernames (from paths)
- Consistent placeholder mapping (`IP_001`, `EMAIL_001`, `PATH_001`, ...)
- `--dry-run`, `--diff`, `--text`, stdin support
- Four output files per run: `_SAFE`, `_REPORT.json`, `_REPORT.csv`, `_MAPPING.json`
- YAML config for custom patterns and disabling detectors
- 86 automated tests

---

## Phase 2 — Professional Tooling 🔧 In Progress

**Batch mode and workflow integration**

- [ ] Batch folder mode (`safetext clean logs/ --output-dir cleaned/`)
- [ ] Recursive folder processing with preserved structure
- [ ] Combined + per-file reports
- [ ] Profile system (VFX, IT, Developer, Medical presets)
- [ ] Cross-run mapping (re-use `_MAPPING.json` across runs)
- [ ] Windows `.exe` standalone packaging (PyInstaller)

---

## Phase 3 — NLP Integration

**Catch what regex misses**

- [ ] spaCy integration for named entity recognition
- [ ] GLiNER integration for domain-specific entity detection
- [ ] Catches: person names, organization names, free-form PII
- [ ] Configurable NLP model selection
- [ ] Optional install (`safetext install nlp`)

---

## Phase 4 — Local AI Integration

**Semantic understanding with local LLMs**

- [ ] Ollama runtime integration
- [ ] Qwen2.5-7B / Qwen3-8B semantic detection
- [ ] Context-aware sensitivity scoring
- [ ] Catches: implicit references, project codenames, domain jargon
- [ ] No cloud — fully offline
- [ ] Optional install (`safetext install ai`)

---

## Phase 5 — Enterprise & Dataset Features

**For teams, pipelines, and research**

- [ ] Dataset anonymization for ML training pipelines
- [ ] Differential privacy support
- [ ] Plugin ecosystem for custom detector modules
- [ ] Enterprise policy system (enforce sanitization rules per team)
- [ ] Audit logging

---

## Supported File Types (Current)

`.txt` `.log` `.json` `.csv` `.yaml` `.ini` `.cfg` `.md` `.xml` `.py` `.js`

---

## Philosophy

**Bad sanitization:**
```
[REDACTED]
[REDACTED]
[REDACTED]
```
Destroys readability, debugging value, and ML usefulness.

**Good sanitization:**
```
SERVER_001
PROJECT_001
CLIENT_001
```
Preserves structure, relationships, and workflow meaning — while removing all identifying information.
