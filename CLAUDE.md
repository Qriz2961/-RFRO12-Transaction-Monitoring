# RFRO XII Transaction Monitoring

LTFRB Regional Franchising and Regulatory Office XII. Single-file app (`index.html`),
Cloudflare Worker + KV backend, deployed on Vercel. Handles franchise transactions,
Confirmation of Units under MC 2026-102, and operator personal data.

## Rule: no `auto` AI routing on this repo

Sessions working here carry operator names, business addresses, plate numbers and
chassis numbers in context. Do not route them through a multi-provider gateway set
to `auto` — that discloses personal data to providers nobody has vetted, which an
agency under the Data Privacy Act (RA 10173) should not be doing. Pin a specific
trusted provider, or do not use a gateway for this repo at all.

## OmniRoute — evaluated 23 Oct 2026

<https://github.com/cheaper-inference/OmniRoute> · MIT · self-hosted AI gateway.

One local endpoint (`localhost:20128/v1`) routing to many providers, with a CLI and
an MCP server. Works with Claude Code, Codex, Cursor and Cline; fails over when a
provider rate-limits. TypeScript/Node, Next.js dashboard, Docker and Electron builds.
Claims 375 providers and 1,400+ models, 150+ free. ~80k stars.

**Maintenance:** healthy — last commit 20 Oct 2026, releases roughly monthly
(v3.8.52 on 19 Oct). Nearly all recent commits come from one account, so treat the
bus factor as 1.

**Keys:** API keys and tokens are AES-256-GCM encrypted with scrypt KDF in SQLite —
*but it falls back to plaintext when `STORAGE_ENCRYPTION_KEY` is unset*. Set that
variable before connecting any provider. Secrets come from env vars; the server
refuses known-weak values. `SECURITY.md` has private disclosure with 48h/5d/14d
targets.

**Privacy:** telemetry off by default. The gateway runs locally and adds no hosted
prompt-processing hop, but prompts still reach the selected upstream provider under
that provider's terms. Opt-in PII redaction, local SQLite audit trail. The
prompt-injection guard is explicitly "not a complete prompt-injection firewall".

**Verdict:** reasonable for personal projects and cost reduction. Not for sessions
touching this repo on `auto`, per the rule above.
