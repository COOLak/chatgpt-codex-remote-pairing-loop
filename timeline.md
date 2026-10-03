# Timeline

All times and dates are UTC.

## 2026-09-26 to 09-29: reports against 1.2026.265 show up on GitHub

Users open issues on OpenAI's Codex tracker describing the same "Authorize this phone" loop on ChatGPT for Android 1.2026.265: [#48555](https://github.com/openai/codex/issues/48555), [#48777](https://github.com/openai/codex/issues/48777) and [#49179](https://github.com/openai/codex/issues/49179). See [public reports](reports.md).

## 2026-09-29, 16:31 UTC: the phone gets 1.2026.265

Google Play updates ChatGPT on the phone to 1.2026.265.

## 2026-10-01: the beta fixes it

Users on [#48555](https://github.com/openai/codex/issues/48555) and [#48777](https://github.com/openai/codex/issues/48777) report that ChatGPT for Android 1.2026.272, from the Google Play beta, pairs normally.

## 2026-10-02: the first OpenAI dead end

An OpenAI Codex session gives up on a broken OneNote. Claude traces it to a preinstalled Intel driver by 15:21 UTC. See [the Intel ICPS incident](https://coolak.github.io/intel-icps-ipv6-flow-label-incident/).

## 2026-10-03, 12:48–12:54 UTC: the loop

The computer generates three fresh pairing codes. The phone completes four sign-ins. Each one ends with "Authorize this phone" again.

## 2026-10-03: the second OpenAI dead end

Asked to look into its own app's pairing loop, ChatGPT declines.

## 2026-10-03, 13:04 UTC: Claude plugs in

Claude installs Android SDK Platform-Tools on the computer. At 13:05 UTC the phone allows USB debugging.

## 2026-10-03, 13:11–13:16 UTC: the loop, recorded

Four more attempts are recorded live from the phone's system log and screen text. Every sign-in succeeds, and every pairing key is deleted 2.6–3.0 seconds later.

## 2026-10-03, about 13:19–14:07 UTC: the investigation

Parallel investigations cover the phone, the computer, the accounts and the public reports. First findings reach me at 13:43 UTC: the app deletes its own pairing key, public reports describe the same loop on 1.2026.265, and users there say 1.2026.272 fixes it. Skeptics try to refute each hypothesis. One survives, and the verdict lands at 14:06 UTC. Claude checks the GitHub reports itself and recommends the update at 14:07 UTC.

## 2026-10-03, 14:08 UTC: the beta sign-up

The ChatGPT listing is opened in Google Play to join the beta. The sign-up looks stuck, but Google Play approves the enrollment.

## 2026-10-03, 14:23–14:24 UTC: fixed

- **14:23:17 UTC:** ChatGPT 1.2026.272 (version code 2627220) is installed.
- **14:23:31 UTC:** the computer shows a fresh pairing code.
- **14:23:58 UTC:** the sign-in returns, the key is kept, and the phone pairs.

## 2026-10-03, 15:24 UTC: published

This tracker is created.
