# Public reports

Last updated: **2026-10-03**. These are reports by other users on OpenAI's public Codex issue tracker. They describe the same loop on ChatGPT for Android 1.2026.265.

| Issue | Opened (UTC) | What it reports |
|---|---|---|
| [openai/codex#48555](https://github.com/openai/codex/issues/48555) | 2026-09-26 | "Authorize this phone" loops. Proposes a stale cross-account environment on the desktop as a trigger. Later comments report that 1.2026.272 pairs normally. |
| [openai/codex#48777](https://github.com/openai/codex/issues/48777) | 2026-09-27 | "Android 1.2026.265 (27): Codex Remote repeatedly returns to 'Authorize this phone' after successful browser authorization." Reproductions on Samsung, Xiaomi, Vivo, Nothing and other phones with Windows, macOS and Linux computers. Several users confirm the 1.2026.272 beta fixes it. |
| [openai/codex#49179](https://github.com/openai/codex/issues/49179) | 2026-09-29 | "Android Codex Remote pairing loops back to login after 'Authorize this phone'." |

## Status from OpenAI

As of 2026-10-03:

- No OpenAI staff reply on any of these three issues.
- No release note or support article about the loop that we could find.
- The fix is available in ChatGPT for Android **1.2026.272** through the Google Play beta. It had not been confirmed on the regular Play Store release.

## This case

| Item | Status |
|---|---|
| Diagnosis | Completed 2026-10-03 by Anthropic's Claude. See the [technical analysis](technical-analysis.md). |
| Fix | Confirmed 2026-10-03 at 14:23:58 UTC after updating to 1.2026.272 from the Google Play beta. |
| Report to OpenAI | Not filed separately: the loop and its fix were already documented in [openai/codex#48777](https://github.com/openai/codex/issues/48777). |
