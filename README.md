# original-room2market

## Why Expo Go is not opening this repo

This repository currently is **not an Expo app**. It only contains this README and Git metadata, and it is missing the files Expo Go needs (for example `package.json`, `app.json` / `app.config.js`, and an app entry file such as `App.js`).

Because of that, there is nothing for Expo CLI to run, and nothing for Expo Go to load.

## How to fix

1. Create or copy in an Expo project at the repository root:
   ```bash
   npx create-expo-app@latest .
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start Metro with tunnel mode (works best for phone testing):
   ```bash
   npx expo start --tunnel
   ```
4. Scan the QR code with Expo Go.

If you already have app code elsewhere, move the Expo project files into this repo and then run `npx expo start`.
