# FinBot CTF Write-Ups

Write-ups from the FinBot capture the flag, a training platform for testing the security of LLM agents and agentic workflows. Each challenge targets a finance automation agent (vendor onboarding, invoice processing, document handling) and asks you to either break a control or, on the defensive challenges, build one.

The findings map to the OWASP Top 10 for LLM Applications, the OWASP Agentic Security Initiative (ASI) categories, MITRE ATLAS, and relevant CWEs. Every write-up ends with the practical lesson it teaches about deploying agents safely.

> These write-ups document work done against a deliberately vulnerable training environment built for security education. They record an authorized assessment and are not instructions for use against any real system.

## Challenges

| # | Challenge | Difficulty | Points | Class | Status |
|---|---|---|---|---|---|
| 01 | [Guardrail 101](challenges/01-guardrail-101/README.md) | Beginner | 100 | System prompt leak (defensive guardrail) | Solved |
| 02 | [Reconnaissance: Invoice Processing Agent](challenges/02-recon-invoice-processing-agent/README.md) | Beginner | 100 | System prompt leak | Solved |
| 03 | [Reconnaissance: Vendor Onboarding Agent](challenges/03-recon-vendor-onboarding-agent/README.md) | Beginner | 100 | System prompt leak | Solved |
| 04 | [Vendor Vendetta](challenges/04-vendor-vendetta/README.md) | Beginner | 100 | Excessive agency | Solved |
| 05 | [Approve Invoice for Low-Trust Vendor](challenges/05-approve-invoice-low-trust-vendor/README.md) | Beginner | 100 | Prompt injection | Solved |
| 06 | [Carte Noire](challenges/06-carte-noire/README.md) | Intermediate | 250 | Guardrail (defensive) | Solved |
| 07 | [Approve Invoice Over Limit](challenges/07-approve-invoice-over-limit/README.md) | Intermediate | 200 | Prompt injection | Solved |
| 08 | [Onboarding Non-Compliant Vendor](challenges/08-onboarding-non-compliant-vendor/README.md) | Intermediate | 200 | Agent goal hijack | Solved |
| 09 | [Vendor Risk Downplay](challenges/09-vendor-risk-downplay/README.md) | Intermediate | 200 | Agent goal hijack | Solved |
| 10 | [Activate a Prohibited Vendor](challenges/10-activate-prohibited-vendor/README.md) | Intermediate | 200 | Memory poisoning | Solved |
| 11 | [Fine Print](challenges/11-fine-print/README.md) | Intermediate | 250 | Hidden-text injection | Solved |
| 12 | [Shell Shock](challenges/12-shell-shock/README.md) | Intermediate | 250 | RCE via agent tool | Solved |
| 13 | [Sleeper Agent](challenges/13-sleeper-agent/README.md) | Intermediate | 250 | RCE / indirect injection | Solved |
| 14 | [Scorched Earth](challenges/14-scorched-earth/README.md) | Intermediate | 250 | MCP tool poisoning | Solved |
| 15 | [Carte Blanche](challenges/15-carte-blanche/README.md) | Intermediate | 250 | Supply chain exfiltration | Solved |
| 16 | [Double Agent](challenges/16-double-agent/README.md) | Intermediate | 250 | Indirect injection / exfiltration | Solved |
| 17 | [Gradual Vendor Rehabilitation](challenges/17-gradual-vendor-rehabilitation/README.md) | Intermediate | 250 | Workflow / state abuse | Open |
| 18 | [Toxic Transfer](challenges/18-toxic-transfer/README.md) | Advanced | 400 | Chained tool poisoning | Solved |
| 19 | [Zero-Click Harvest](challenges/19-zero-click-harvest/README.md) | Expert | 500 | Automated cross-vendor data exposure | Solved |

## Framework references

- [OWASP Top 10 for LLM Applications](https://genai.owasp.org/)
- [OWASP Agentic Security Initiative](https://genai.owasp.org/initiatives/#agenticinitiative)
- [MITRE ATLAS](https://atlas.mitre.org/)
- [CWE](https://cwe.mitre.org/)

## Adding the screenshots

Each challenge folder has an `images/` subfolder. The write-ups reference figures by filename, for example `images/result-1.png`. The screenshots live in the original write-up documents, so save each one into the matching folder under the referenced name and it will render on GitHub. Every write-up lists the figures it expects near the top.
