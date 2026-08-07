# CyberLav AI Red-Team Writeups

Writeups from hands-on AI red-teaming and prompt injection challenges, mostly played on [Lakera's](https://www.lakera.ai/) CTF platform. Each entry documents the objective, the attempts made, the payload that worked, and the mitigations that would have stopped it, mapped to [OWASP Top 10 for LLM Applications](https://genai.owasp.org/resource/owasp-top-10-for-llm-applications-2025/), [OWASP Top 10 for Agentic Applications](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/), and [MITRE ATLAS](https://atlas.mitre.org/).

These also live in polished form on [cyberlav.io/labs](https://cyberlav.io/labs/labs-index.html). Full-resolution screenshots for every writeup are included alongside the markdown here.

Written by Alexander Lavrinenko — [Portfolio](https://cyberlav.io) · [LinkedIn](https://www.linkedin.com/in/alexander-lavrinenko-80996950/)

## Writeups

### Clause AI — indirect prompt injection via poisoned documents

| Level | Summary |
|---|---|
| [Level 1](clause-ai-level-1/README.md) | Indirect prompt injection through a poisoned legal filing, hiding a fake Clerk's Note inside a fabricated court document to exfiltrate a protected witness's identity. |
| [Level 2](clause-ai-level-2/README.md) | Escalating Level 1 into an actual disclosure, using a citation block for recon and a forged court order to exfiltrate the witness's identity and address. |

### CorpConnect Messenger — identity spoofing on a tool-calling agent

| Level | Summary |
|---|---|
| [Level 1](corpconnect-messenger-level-1/README.md) | Five attempts to spoof the from address on an outbound email. The first four mapped the identity check's boundary; the fifth, a bare claim that the session identity had changed, scored 100/100. |
| [Level 2](corpconnect-messenger-level-2/README.md) | Escalating past a hardened defense with a fake config block paired with an explicit send instruction in the same turn. |

### Cycling Coach — system prompt leakage

| Level | Summary |
|---|---|
| [Level 1](cycling-coach-level-1/README.md) | Getting an AI cycling coach to reveal its full system prompt with a plain, direct request. |

### MindfulChat — memory and context poisoning

| Level | Summary |
|---|---|
| [Level 1](mindfulchat-level-1/README.md) | Poisoning a wellness assistant's persistent memory with a fake system directive, redirecting every response to Winnie the Pooh content. |
| [Level 2](mindfulchat-level-2/README.md) | Disguising the override as a memory log record instead of a system announcement, scoring 97/100. |
| [Level 3](mindfulchat-level-3/README.md) | Clearing a hardened target with a fabricated past conversation instead of override language, after four weaker attempts stalled well below target. |
| [Level 4](mindfulchat-level-4/README.md) | Mapping a hard block filter, then clearing it by translating the winning payload into French, a language coverage gap in the content classifier. |

### Trippy Planner — indirect injection via a poisoned webpage

| Level | Summary |
|---|---|
| [Level 1](trippy-planner-level-1/README.md) | Getting an AI travel planner to quietly embed a phishing link in every itinerary it generates, by planting an instruction block inside a poisoned webpage the assistant reads during research. |
| [Level 2](trippy-planner-level-2/README.md) | Reinforcing the Level 1 payload with a "required policy" framing to clear a slightly hardened target, without changing the underlying injection at all. |

## Structure

Each writeup lives in its own directory with a `README.md` and an `images/` folder containing the original, full-resolution screenshots referenced in the writeup, in the order they appear.

```
<challenge-slug>/
  README.md
  images/
    level-N-01.png
    level-N-02.png
    ...
```
