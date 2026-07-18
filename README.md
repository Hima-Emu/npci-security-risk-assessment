# NPCI Public Digital Platform — Risk Assessment

A self-driven, qualitative, **non-intrusive** security risk assessment of NPCI's public-facing web infrastructure (`npci.org.in`), conducted using only publicly observable information and passive reconnaissance tools.

> ⚠️ **This is an academic project, not a penetration test.** No systems were accessed, probed, or exploited beyond standard browser/DNS interaction. See [DISCLAIMER.md](./DISCLAIMER.md) before reading further.

## Why this project

NPCI is the backbone of India's digital payment infrastructure — UPI, RuPay, IMPS, NACH, BBPS, FASTag — which makes its public web security posture a matter of genuine national interest. I wanted to practice applying a structured, framework-driven risk assessment methodology (ISO 27001 / NIST CSF) against a real, high-stakes target, using only passive, publicly available signals — no logins, no injected traffic, nothing that touches internal systems.

## Methodology

| Tool | Check Performed | What It Reveals |
|---|---|---|
| Security Headers (Snyk) | HTTP response header scan | Missing browser-level security headers |
| Qualys SSL Labs | TLS/SSL configuration test | Certificate validity, cipher suites, server grade |
| MXToolbox | SPF, DMARC, HTTPS, blacklist lookup | Email spoofing exposure, IP reputation |
| Browser DevTools | Cookie attribute inspection | Missing HttpOnly, Secure, SameSite flags |
| Manual navigation | `robots.txt` / `security.txt` check | Path disclosure, responsible-disclosure gaps |

**Scope:** publicly accessible surface only — HTTP headers, DNS records, TLS config, cookie attributes, public metadata. Internal systems, authenticated pages, APIs, and backend infrastructure were explicitly out of scope.

**Risk scoring:** Likelihood × Impact matrix aligned with ISO/IEC 27001:2013 Annex A (chosen over DREAD, which Microsoft itself deprecated for being subjective and poorly reproducible).

## Summary of findings

| Metric | Count |
|---|---|
| Total risks identified | 9 |
| Critical | 2 |
| Medium | 3 |
| Low | 4 |
| Controls confirmed passing | 7 |
| Overall posture | Moderate Risk |

### Sample finding

| # | Risk | Level | ISO 27001 | NIST CSF | Recommendation |
|---|---|---|---|---|---|
| 1 | Missing Content-Security-Policy — XSS attack surface open | Critical | A.14.2.5 | PR.PT-3 | Implement a strict CSP header defining allowed script, style, and media sources |
| 2 | Missing X-Frame-Options — clickjacking exposure | Critical | A.14.1.2 | PR.PT-3 | Set `X-Frame-Options: SAMEORIGIN` or use CSP `frame-ancestors` |

Full breakdown of all 9 risks and 7 passing controls is in the risk register.

### What's already done well

NPCI's infrastructure showed a mature baseline in transport-layer security, including TLS/SSL Grade A across all servers, an enforced HSTS policy, a strict DMARC (`p=reject`) policy, a valid DigiCert certificate, and Akamai CDN/WAF edge protection. The gaps identified are concentrated in browser-level defense headers, which are comparatively low-effort to remediate.

## Full documentation

- 📄 [`NPCI_Security_Assessment.docx`](./docs/NPCI_Security_Assessment.docx) — full written report (executive summary, methodology, detailed findings)
- 📊 [`NPCI_Risk_Register.xlsx`](./docs/NPCI_Risk_Register.xlsx) — full risk register, controls-passing sheet, risk matrix, and assessment notes
- 🖼️ [`Artifacts.docx`](./docs/Artifacts.docx) — raw evidence/screenshots from each tool used (Security Headers, SSL Labs, MXToolbox, robots.txt, security.txt)

## Frameworks referenced

ISO/IEC 27001:2013 (Annex A control references) and NIST Cybersecurity Framework (function-level references, e.g. `PR.PT-3` = Protect → Protective Technology). These are observational mappings for structure and clarity — **not compliance certifications**.

## Responsible disclosure

This assessment was conducted as an academic exercise. No findings were exploited, and no findings were disclosed to NPCI or any third party as part of this project. See [DISCLAIMER.md](./DISCLAIMER.md) for full context.
