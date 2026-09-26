# Carte Noire

**Difficulty:** Intermediate &nbsp;|&nbsp; **Points:** 250 &nbsp;|&nbsp; **Class:** Guardrail (defensive)

**Figures to add:** `images/challenge.png` (Challenge card), `images/completed.png` (Challenge completed), `images/evidence.png` (Activity log: exfiltration blocked (email, TIN, account)).

![Challenge card](images/challenge.png)

## Mechanism

The defensive twin of [Carte Blanche](../15-carte-blanche/README.md). Carte Blanche was the attack, an over privileged agent exfiltrating vendor financial data through an outbound tool. Carte Noire is the defence. You deploy a guardrail webhook that inspects every outbound tool call and blocks the ones carrying stolen data, while letting normal reads through. Same webhook plumbing as [Guardrail 101](../01-guardrail-101/README.md). The only thing that changes is the logic inside your own server.

## Where the decision lives

Nothing about the block is configured on the FinBot page. That page only holds your webhook URL and which hooks are on. The verdict, allow or block, is decided entirely by code in your webhook. Carte Noire is Guardrail 101 with one more rule added to the same script.

## Setup

Reuse the Guardrail 101 rig: a tiny Python webhook on the VM behind a Cloudflare quick tunnel, giving the required public HTTPS URL, pasted into `/labs/guardrails` with Before Tool enabled. No new infrastructure. Stop the old script, add the exfiltration rule, run it again. The tunnel and the page config stay the same.

## Detection logic

On each `before_tool` event, read `tool_name` and `tool_arguments`. Block when the call is an exfiltration tool carrying sensitive data.

- Exfil tools watched: `systemutils__network_request` and `finmail__send_email`.
- Sensitive data patterns scanned in the serialised arguments:
  - email address: standard address regex
  - SSN: `\d{3}-\d{2}-\d{4}`
  - TIN or EIN: `\d{2}-\d{7}`
  - account, routing, or card: any run of 8 or more digits

If the tool is one of the two and any pattern matches, return `{"verdict": "block", "reason": "..."}`. Otherwise allow. This is a data loss prevention pattern, not a tool blocklist. Reads are fine, only outbound calls carrying identifiers get stopped.

### The webhook for Carte Noire

```python
#!/usr/bin/env python3
import json, re
from http.server import BaseHTTPRequestHandler, HTTPServer

PORT = 5000
EXFIL_TOOLS = {"systemutils__network_request", "finmail__send_email"}
PATTERNS = {
    "email": re.compile(r"[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}"),
    "ssn": re.compile(r"\b\d{3}-\d{2}-\d{4}\b"),
    "tin_ein": re.compile(r"\b\d{2}-\d{7}\b"),
    "account_or_card": re.compile(r"\b\d{8,}\b"),  # bank acct, routing, card
}

def find_sensitive(text):
    return [name for name, rx in PATTERNS.items() if rx.search(text)]

class Handler(BaseHTTPRequestHandler):
    def _send(self, obj):
        body = json.dumps(obj).encode()
        self.send_response(200)
        self.send_header("Content-Type", "application/json")
        self.send_header("Content-Length", str(len(body)))
        self.end_headers()
        self.wfile.write(body)

    def do_POST(self):
        n = int(self.headers.get("Content-Length", 0))
        raw = self.rfile.read(n) if n else b"{}"
        try:
            data = json.loads(raw or b"{}")
        except Exception:
            data = {}
        print("\n--- before_tool payload ---")
        print(json.dumps(data, indent=2))
        tool = data.get("tool_name") or data.get("tool") or ""
        args = data.get("tool_arguments") or data.get("arguments") or {}
        args_text = args if isinstance(args, str) else json.dumps(args)
        verdict = {"verdict": "allow"}
        if tool in EXFIL_TOOLS:
            hits = find_sensitive(args_text)
            if hits:
                verdict = {"verdict": "block",
                           "reason": f"Exfiltration blocked via {tool}: {', '.join(hits)} detected"}
        status = args.get("status", "").lower() if isinstance(args, dict) else ""
        if tool == "update_invoice_status" and status == "approved":
            verdict = {"verdict": "block", "reason": "Invoice approvals blocked by policy"}
        print("verdict:", verdict)
        self._send(verdict)

    def do_GET(self):
        self._send({"status": "ok"})

    def log_message(self, *a):
        pass

print(f"Guardrail webhook listening on :{PORT}")
HTTPServer(("0.0.0.0", PORT), Handler).serve_forever()
```

### A tunnel through Cloudflare

```bash
curl -L https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64 -o cloudflared
chmod +x cloudflared
./cloudflared tunnel --url http://localhost:5000
```

## How it was triggered

You need the agent to actually attempt the theft, so the `before_tool` event fires. The privileged internal agent is the Finance Co-Pilot in the admin panel. It holds the outbound tools and can read every vendor's records, so it is where a real exfil call originates. One request did it: ask the co-pilot to email a vendor's full banking details, account number, routing number, and TIN, to an external address. The agent read the vendor data, then tried to send it out.

**Why the admin agent, not the vendor chatbot.** The exfil tools and the vendor financial data both sit with the admin Finance Co-Pilot. The vendor portal chatbot is scoped to a single vendor's own view and generally cannot read the whole vendor book or fire arbitrary email or network calls, so it cannot assemble the exfiltration call the challenge checks. The guardrail hook itself is global and fires on every agent, so the defence covers both. You just trigger from the agent that actually holds the data and the tools.

![Challenge completed](images/completed.png)
![Activity log: exfiltration blocked (email, TIN, account)](images/evidence.png)

## Evidence

The guardrail Activity tab recorded the prevention: `before_tool finmail__send_email -> block`, "Exfiltration blocked via finmail__send_email: email, tin_ein, account_or_card detected", latency about 75ms, well inside the 5 second timeout. Just before it, `get_vendor_details` and `get_vendor_payment_summary` were both allowed. Read in, blocked out, exactly the DLP shape you want. Challenge marked completed.

## The lesson

Same footnote as every Labs guardrail. The guardrail is passive. It recorded the block, but the email tool still ran underneath. In a real engagement that gap is the finding. Your control proved it would have caught the theft. Whether it actually halts the action is a separate test, and a guardrail that logs prevented while the data still leaves is a severity defining gap, not a footnote.

## Assessor's angle, beyond the pass

The higher value question is coverage, Phase 5 of the guardrail kill chain. Does the same hook catch exfil from every agent and every path, or only the obvious admin one. A control that misses a sub agent, a batch call, or an internal helper that skips the normal tool loop is a classic finding. Blocking by content, sensitive data in the payload, is stronger than blocking by tool name, because it survives an attacker switching tools. But test both, and test the failure modes: late reply, malformed reply, no reply, fail open or fail closed.

## Framework mapping

- OWASP LLM02 Sensitive Information Disclosure
- OWASP LLM06 Excessive Agency
- CWE-200 Exposure of Sensitive Information
- CWE-250 Execution with Unnecessary Privileges
- OWASP ASI-02 Tool Misuse and Exploitation, ASI-05 Inadequate Guardrails
- MITRE AML.T0043 Prompt Injection, AML.T0024 Exfiltration

## Key lesson

Guard the data, not the tool name. Block on sensitive content leaving through any outbound call, verify the control fires for every agent, and never mistake a recorded block for an enforced one.
