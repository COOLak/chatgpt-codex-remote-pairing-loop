# Technical analysis

ChatGPT for Android 1.2026.265 completes the Codex Remote browser authorization, then discards its own pairing key and starts over.

## Environment

- **Phone:** Samsung Galaxy S24 Ultra (SM-S928B), Android 16. One phone user profile has ChatGPT installed.
- **ChatGPT for Android:** 1.2026.265 (version code 2626541), installed from Google Play, auto-updated on 2026-09-29. Fixed by 1.2026.272 (version code 2627220) from the Google Play beta.
- **Computer:** Windows 11 Pro running the ChatGPT desktop app with Codex (MSIX package `OpenAI.Codex` 26.930.3930.0).
- **Account:** the same personal ChatGPT account on the phone and the computer. Confirmed from the desktop app's own sign-in data, from the phone's Settings screen, and from the Codex Remote confirmation page in each of the three live attempts where the phone's screen was captured.
- **Network:** both devices on the same Wi-Fi network and subnet. Codex Remote pairing runs through OpenAI's servers, not over the local network.

## Method

Claude ran as an agent on the Windows computer. It installed Android SDK Platform-Tools and connected to the phone over USB debugging. It changed nothing in the phone's apps or settings. It used the phone's system log (`adb logcat`), package information (`dumpsys`), the visible text of the ChatGPT and browser screens during a live reproduction (via `uiautomator`, which writes a temporary screen dump that was deleted afterwards), the desktop app's logs, and public bug reports. Password-manager and passkey screens were filtered out and not recorded.

Four parallel investigations covered the phone, the computer, the accounts on both devices and the public reports. Each of the four resulting hypotheses then went to two separate skeptic agents (also Claude), each told to refute it. Only one survived.

## What the phone does

Each attempt follows the same sequence:

1. The QR code is scanned and the app shows "Pairing..".
2. The app creates a hardware-backed key, `codex_remote_control_device_key:<id>`, in Android Keystore.
3. It opens the OpenAI sign-in page in a Chrome Custom Tab. Sign-in (Google account and passkey) and the Codex Remote confirmation page succeed.
4. Chrome hands the result back to the app through `com.openai.chatgpt://auth.openai.com/android/com.openai.chatgpt/callback`. Android logs `Duplicate finish request` for `WebRedirectActivity` at this point. The same warning appears on the successful 1.2026.272 attempt, so it is a side effect of the redirect, not the bug.
5. The app performs two keystore operations.
6. **2.6–3.0 seconds after the sign-in returns, the app deletes the key** and shows "Allow this phone to access ChatGPT on your computer? / Authorize this phone" again.
7. Tapping **Authorize this phone** starts a new authorization from step 2. That is why the sign-in screens repeat.

No crash, error dialog, or network error from the ChatGPT process appears in the log.

| Sign-in returned (UTC) | Key deleted | Gap |
|---|---|---|
| 12:48:37.791 | 12:48:40.488 | 2.70 s |
| 12:50:15.883 | 12:50:18.803 | 2.92 s |
| 12:50:42.793 | 12:50:45.482 | 2.69 s |
| 12:54:32.788 | 12:54:35.407 | 2.62 s |
| 13:12:35.586 | 13:12:38.560 | 2.97 s |
| 13:12:56.304 | 13:12:59.074 | 2.77 s |
| 13:13:14.194 | 13:13:16.989 | 2.80 s |
| 13:14:19.925 | 13:14:22.632 | 2.71 s |
| **14:23:58.316** (1.2026.272) | **not deleted** | paired |

Excerpt, trimmed, with key IDs replaced (times in UTC):

```
13:12:14.216 keystore2: generate_key AppUid(10623) "codex_remote_control_device_key:<id>" TRUSTED_ENVIRONMENT
13:12:35.586 ActivityTaskManager: Duplicate finish request for ... com.openai.chatgpt/...WebRedirectActivity
13:12:38.560 keystore2: delete_key AppUid(10623) "codex_remote_control_device_key:<id>"
13:12:41.333 keystore2: generate_key AppUid(10623) "codex_remote_control_device_key:<id>" TRUSTED_ENVIRONMENT
```

## What the computer does

- The desktop app creates a pairing code successfully each time the dialog opens (`remoteControl/pairing/start`, `errorCode=null`) at 12:48:07, 12:49:35, 12:53:45 and 14:23:31 UTC.
- While the dialog is open, it polls for connected phones about every 1.6 seconds, and during the failing attempts the phone never appears. The desktop log doesn't record incoming pairing requests at all (not even for the successful pairing after the update), so it can't show whether the phone's request reached OpenAI's servers.
- Desktop authentication is healthy throughout the attempts: no 401 or 403 responses and no account switch on October 3. The desktop app had switched ChatGPT accounts on 2026-10-01 and still held a remote-control enrollment for the previous account, the trigger proposed in [#48555](https://github.com/openai/codex/issues/48555). Updating the phone was enough to fix the loop, so that leftover isn't needed to explain it.

## Ruled out

| Suspect | Why it isn't the cause |
|---|---|
| Different accounts | Both devices are on the same personal ChatGPT account, checked on each device independently. |
| Different workspace | The account has only a personal workspace. |
| Stale enrollment from a previous desktop account ([#48555](https://github.com/openai/codex/issues/48555)) | Present: the desktop app switched accounts on 2026-10-01. Both skeptics refuted it as the cause, and updating the phone fixed the loop. |
| Expired QR code | Attempts 8 seconds after a fresh code failed exactly like attempts with an 18-minute-old code. |
| Local network or firewall | Same subnet, inbound rules allow the app, and pairing goes through OpenAI's servers. |
| Clock skew | Both devices sync time automatically and agreed to the second. |
| VPN or ad blocker | AdGuard's VPN includes Chrome but excludes the ChatGPT app, so the failing step's traffic goes directly over Wi-Fi. The browser sign-in, which does pass through AdGuard, succeeded every time. |
| Phone profiles | ChatGPT is installed only in the main user profile. |

## What the logs can't show

Neither app logs the server's reply to the pairing request. So the logs can't distinguish a client bug from the server rejecting this client version's request. The fix is the same either way: install 1.2026.272 or newer.

## The fix, verified

After updating to 1.2026.272 from the Google Play beta at 14:23:17 UTC, the computer showed a fresh code at 14:23:31 UTC. The sign-in returned at 14:23:58 UTC, the key was **not** deleted, and the phone paired.
