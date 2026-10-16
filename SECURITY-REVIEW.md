# Security Review

Reviewed: `index.html` (GitHub token handling and post publishing)

| # | Severity | File | Lines | Vulnerability | Confidence |
|---|----------|------|-------|---------------|------------|
| 1 | 🟠 HIGH | `index.html` | 832, 877, 926, 934-936 | Stored cross-site scripting through post content can execute in the site origin. The GitHub token is stored in `localStorage`, so injected JavaScript can read and potentially exfiltrate it. | 8/10 |

## Details

Post content is interpolated into generated HTML without escaping and later rendered using `innerHTML`. If attacker-controlled content is published, it can execute JavaScript in visitors' browsers. The page stores the GitHub token in `localStorage`, making it accessible to same-origin scripts, including injected scripts.

## Recommended remediation

- Treat post content as text by escaping it or rendering it with `textContent`; if HTML is required, sanitize it with a strict allowlist.
- Avoid storing a repository-write token in browser `localStorage`. Prefer a server-side endpoint that keeps credentials out of browser JavaScript, or use a narrowly scoped, short-lived credential with an explicit threat model.
