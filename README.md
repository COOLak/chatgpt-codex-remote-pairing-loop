# ChatGPT couldn't pair with ChatGPT. It took Claude to find out why.

> **Status, October 3, 2026: resolved.** On ChatGPT for Android 1.2026.265, pairing the phone with Codex on a computer can loop forever. Version **1.2026.272**, from the Google Play beta, fixes it. If you're stuck, [here's the fix](fix.md).

My phone and my laptop were signed into the same ChatGPT account, and pairing them for **Codex Remote** went in circles: scan the QR code, sign in, approve, "Authorize this phone", and straight back to sign-in. No error anywhere. The laptop just kept waiting.

ChatGPT wouldn't look into its own app. So Anthropic's **Claude**, already running as an agent on the laptop, installed Android's platform tools, connected to the phone over USB, and read its logs. Under an hour later it had the answer: after every successful sign-in, the ChatGPT app **deleted its own pairing key 2.6–3.0 seconds later** and started over. That matches a known, public bug in ChatGPT for Android 1.2026.265 that OpenAI hadn't acknowledged as of October 3. One beta update later, the sign-in came back at 14:23:58 UTC and the phone paired seconds later.

## Start here

- **[Read the story](https://coolak.github.io/chatgpt-codex-remote-pairing-loop/)**
- **[Stuck in the loop? The fix](fix.md)**
- **[Technical analysis: logs, timings and what was ruled out](technical-analysis.md)**
- **[Public reports on OpenAI's tracker](reports.md)**
- **[Dated timeline](timeline.md)**
- **[Machine-readable state](incident-state.json)**

## Short summary

| | |
|---|---|
| **Component** | ChatGPT for Android 1.2026.265 (version code 2626541), Codex Remote pairing |
| **Phone** | Samsung Galaxy S24 Ultra (SM-S928B), Android 16 |
| **Computer** | Windows 11 Pro, ChatGPT desktop app with Codex (`OpenAI.Codex` 26.930.3930.0) |
| **Defect** | The browser authorization succeeds, then the app deletes its `codex_remote_control_device_key` 2.6–3.0 s later and asks to "Authorize this phone" again. 8 of 8 recorded sign-ins looped. |
| **Not the cause** | Account mismatch, workspace, expired QR code, network, firewall, clock, VPN, phone profiles |
| **Fix** | ChatGPT for Android **1.2026.272** (Google Play beta). Paired on the first try. |
| **Diagnosed by** | Anthropic's Claude, running as an agent on the laptop, with the phone connected over ADB |

## Not the first time

The day before, an OpenAI Codex session gave up on a OneNote failure that had lasted nearly three weeks. Claude traced it to a preinstalled Intel driver: [Intel ICPS breaks IPv6, OneNote and Microsoft 365](https://github.com/COOLak/intel-icps-ipv6-flow-label-incident). This makes two OpenAI dead ends and two Claude diagnoses, solved on October 2 and October 3, 2026.

Credit where it's due: the fix itself is OpenAI's 1.2026.272 build. ChatGPT just didn't tell me about it.

## Privacy

Email addresses, account and device identifiers, network names and addresses are omitted. Log excerpts are trimmed, and key IDs are replaced with `<id>`.
