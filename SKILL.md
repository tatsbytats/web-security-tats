---
name: web-security-tats
description: >
  Use when building, reviewing, or hardening any full-stack web app, API, or
  deployment. Distills the essential web-security mindset, the OWASP Top 10,
  concrete "never do this" rules, a layered secure architecture, and a concrete
  audit workflow an AI coding agent can run. Agent-agnostic (Claude Code,
  opencode, Codex, Cursor, etc.).
---

# Full-Stack Web Security (essentials)

Security is not "make it 100% secure." It is: reduce attack surface, detect
problems, limit damage. Apply one habit to every feature:

> "What happens if the user intentionally sends something malicious?"
> Can they spam it, inject, send 10M requests, send a huge payload, read
> another user's data, bypass auth, manipulate the DB, or use it for SSRF?

## The non-negotiable rules (the "never"s)

- **Passwords: hash, never encrypt or store plaintext.** Use Argon2 or bcrypt
  with a per-user salt. Encryption is reversible; hashing is the point.
- **Never put secrets in frontend code.** Anything shipped to the browser is
  visible. Keys belong in env vars / a secret manager, never in `.js` bundles.
- **Never build SQL by string concatenation.** Use parameterized queries /
  prepared statements / the ORM's bind API. String-built SQL = SQL injection.
- **Never assign raw user input to `innerHTML`.** Use `textContent` / safe DOM
  APIs; encode output and sanitize input.
- **Never trust user input.** Validate (is it allowed?) and sanitize (can I
  safely transform it?) for every text/number/date/email/URL/file/JSON/header.
- **Never log secrets or stack traces to users.** Log passwords, tokens,
  cookies, card numbers, API secrets only server-side, redacted. Show users a
  generic message + error ID (e.g. `ERR-82F91`).
- **JWT does NOT auto-fix CSRF.** Use CSRF tokens / SameSite cookies / Origin
  checks where state-changing requests are vulnerable.
- **Authorization ≠ authentication.** Logging in is not enough — check
  *resource ownership* (IDOR/BOLA): a user must not read `/api/users/124`
  just by changing an id from `123`. Enforce object-level and function-level
  authz, watch for horizontal & vertical privilege escalation.
- **A secret committed then deleted still lives in Git history.** Rotate it
  and purge history (e.g. `git filter-repo` / BFG).
- **Verify uploaded files by content, not extension.** `.jpg` may be malware.
  Check type + MIME + size, randomize filename, isolate storage, scan.
- **Containers: don't run as root.** Use minimal base images, non-root user,
  read-only FS where possible.

## OWASP Top 10 (the core study area)

Broken Access Control · Cryptographic Failures · Injection · Insecure Design ·
Security Misconfiguration · Vulnerable Components · Authentication Failures ·
Software/Data Integrity Failures · Logging & Monitoring Failures · SSRF.
For each feature, ask "how could someone abuse this?"

## Highest-value subset (learn these first)

OWASP Top 10 + authentication/authorization + SQL injection + XSS + CSRF +
API security + secrets management + Linux/server security.

## Layer your defenses

For every request: HTTPS → WAF → rate limiting → authentication →
authorization → input validation → business logic → parameterized DB queries
→ least-privilege DB account.

Target shape: Internet → Cloudflare (WAF/DDoS) → Nginx (reverse proxy, HTTPS)
→ Frontend (React/Next) → API (Node/Express) → PostgreSQL + Redis. Don't expose
DB/cache ports (3306/5432/6379) publicly; isolate on a private network.

## Learning progression (condensed)

1. **Foundation:** HTML/CSS/JS, HTTP(S), cookies, sessions, CORS, SOP.
2. **Backend:** Node/Express, REST, middleware, auth, authz, validation, errors.
3. **Database:** SQL, parameterized queries, transactions, least-privilege perms.
4. **Security:** OWASP Top 10, XSS, CSRF, SQLi, IDOR, SSRF, rate limiting.
5. **Production:** Linux, Nginx, HTTPS/HSTS, firewall, Docker, env/secrets,
   logging, monitoring, backups.
6. **Advanced:** OAuth/OIDC (+PKCE), WebSockets auth, cloud IAM, CI/CD security,
   SAST/DAST, pentesting, threat modeling.

## Other critical surfaces (one-line each)

- **Dependencies:** `npm audit`, lockfiles, pinning, watch transitive deps and
  supply-chain/typosquatting. Update and scan (SCA).
- **OAuth/social login:** use a vetted library; Authorization Code + PKCE;
  validate redirect URIs; use `state`/`nonce`; don't hand-roll.
- **WebSockets:** auth the connection, validate Origin, validate messages,
  rate-limit, enforce room authorization.
- **SSR:** keep server secrets out of client bundles; beware template
  injection and SSRF in server actions.
- **CI/CD & Git:** `.gitignore` secrets, branch protection, Dependabot, Actions
  secret hygiene, SAST/DAST/container scanning, env separation.
- **Payments:** route through a gateway; verify webhook signatures; idempotency;
  never store raw card data.
- **Cloud:** IAM least-privilege, security groups, private subnets, signed URLs,
  DDoS/CDN protection.
- **Testing:** attack your own app with ZAP, Burp, Nmap, fuzzing; verify fixes.

## Audit workflow (agent-executable)

When asked to review or harden a project, an agent should:

1. **Identify the stack** (language/framework, DB, infra, secret locations).
2. **Run dependency & secret scans:** `npm audit` / `pip-audit`; secret
   scanners (gitleaks) and a Git-history grep for leaked keys.
3. **Static checks:** SAST (semgrep), and grep for `innerHTML =`, `eval(`,
   `child_process.exec(`, and string-built `SELECT`.
4. **Manual rule checks:** password hashing, no frontend secrets, parameterized
   SQL, ownership-based authz (IDOR), input validation, safe error handling,
   file-upload content checks, CSRF where needed, non-root containers.
5. **Report** Critical (exploitable now) / High (real risk) / Recommendations,
   each with file:line, and note anything that could not be verified
   (e.g. live TLS needs a deployed URL). Do not edit code unless asked.
