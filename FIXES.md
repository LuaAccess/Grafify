# Security Audit Fixes — LuaAccess/Grafify

## Fix 1 — Cross-contaminated URLs in pyproject.toml (REQUIRED)

**File:** `pyproject.toml`  
**Lines:** 46–48

```diff
- Homepage   = "https://github.com/LuaAccess/graphify"
- Repository = "https://github.com/LuaAccess/graphify"
- Issues     = "https://github.com/LuaAccess/graphify/issues"
+ Homepage   = "https://github.com/LuaAccess/Grafify"
+ Repository = "https://github.com/LuaAccess/Grafify"
+ Issues     = "https://github.com/LuaAccess/Grafify/issues"
```

URLs pointed to a non-existent `LuaAccess/graphify` repo.
Corrected to `LuaAccess/Grafify` (this repo).

---

## Confirmed Clean (no changes needed)

| Area | Status | Notes |
|---|---|---|
| GitHub Actions | ✅ SHA-pinned | `checkout@11bd71901b`, `setup-uv@6b9c6063ab` |
| CI permissions | ✅ `contents: read` | Read-only, no write/admin |
| Dependabot | ✅ Present | pip + github-actions weekly |
| Telemetry | ✅ None | No telemetry.py, no phone-home calls |
| `shell=True` | ✅ Absent | Explicitly avoided per SECURITY.md |
| `eval`/`exec` | ✅ Absent | tree-sitter AST only, no code execution |
| Hardcoded credentials | ✅ None | API key via env var only (`GRAPHIFY_API_KEY`) |
| SSRF protection | ✅ Strong | `security.py` DNS rebinding guard, redirect re-validation |
| Path traversal | ✅ Blocked | `validate_graph_path()` enforces graphify-out/ boundary |
| XSS in HTML output | ✅ Mitigated | `sanitize_label()` + `sanitize_metadata()` |
| Prompt injection via labels | ✅ Mitigated | sanitizer applied to MCP text output |
| SKILL.md | ✅ Clean | No prompt injection, no exfiltration, valid frontmatter |

---

## Advisory — Version Gap

Your fork is at `0.8.35`. Upstream `v8` branch is at `0.8.44`.  
9 patch versions behind. Sync recommended when convenient.

Upstream: https://github.com/safishamsi/graphify/tree/v8
