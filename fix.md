# Stuck in the "Authorize this phone" loop?

This applies to **ChatGPT for Android** when pairing with Codex on a computer through **Codex Remote** ("Connect computer" in the app). The computer can run Windows, macOS or Linux.

## Typical symptoms

- You scan the pairing QR code, sign in, and confirm your account on the "Confirm this is the ChatGPT account you want to use with Codex Remote" page.
- The app shows "Pairing..", then "Allow this phone to access ChatGPT on your computer?" with an **Authorize this phone** button.
- Tapping it sends you back to the sign-in page, and the whole thing repeats.
- No error message appears on the phone or on the computer. The computer never lists the phone.

## 1. Check your ChatGPT version

On the phone: **ChatGPT → Settings → About**. Or, with the phone connected over USB debugging:

```
adb shell dumpsys package com.openai.chatgpt | findstr versionName
```

(`grep` instead of `findstr` on macOS and Linux.) If it says **1.2026.265**, you are most likely hitting this bug.

## 2. Install the fixed build

1. Open the **Google Play Store** and go to the **ChatGPT** listing.
2. Scroll down to the beta section and join the beta. It may look stuck. Give it a few minutes; Play confirms enrollment separately.
3. Come back to the listing and **tap Update**. Joining the beta does nothing until you install the update.
4. Confirm the version is **1.2026.272 or newer**.

Once a regular (non-beta) release at 1.2026.272 or newer reaches your phone, a normal Play Store update should do the same job.

Don't install ChatGPT APK files from third-party sites.

## 3. Pair again with a fresh code

1. On the computer, close the pairing dialog and open it again, so it shows a **new** QR code.
2. Keep the computer's ChatGPT window open.
3. On the phone, open **ChatGPT → Codex → Connect computer → Start pairing**, scan, sign in, and tap **Authorize this phone** once.
4. Wait about ten seconds. The phone should show the computer as connected.

## Still looping on 1.2026.272 or newer?

Try these one at a time, each with a fresh QR code:

- Check that the computer and the phone are signed into the **same ChatGPT account and workspace**.
- If the computer's ChatGPT app was recently switched to a different ChatGPT account, see [openai/codex#48555](https://github.com/openai/codex/issues/48555).
- On the computer, turn remote connections off and on again, then generate a new code.
- If you use a VPN or ad blocker on the phone, pause it while you pair.

Then add your phone model, Android version, ChatGPT version and computer to the [tracking issue](https://github.com/COOLak/chatgpt-codex-remote-pairing-loop/issues/1) or to [openai/codex#48777](https://github.com/openai/codex/issues/48777). Don't post email addresses, account IDs or pairing codes.
