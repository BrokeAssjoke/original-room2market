# original-room2market

## Why Expo Go is not opening this repo

This repository currently is **not an Expo app**. It only contains this README and Git metadata, and it is missing the files Expo Go needs (for example `package.json`, `app.json` / `app.config.js`, and an app entry file such as `App.js`).

Because of that, there is nothing for Expo CLI to run, and nothing for Expo Go to load.

## How to fix (bootstrap the Expo app first)

1. Create or copy in an Expo project at the repository root:
   ```bash
   npx create-expo-app@latest .
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start Metro with tunnel mode (best first test from a physical phone):
   ```bash
   npx expo start --tunnel
   ```
4. Scan the QR code with Expo Go.

If you already have app code elsewhere, move the Expo project files into this repo and then run `npx expo start`.

## Why fetch/proxy fails in Expo Go (after the app exists)

If Expo Go opens but API calls fail, the most common cause is that your app is trying to fetch through an address Expo Go cannot reach.

### Common root causes

1. **Using `localhost` in mobile fetch URLs**
   - In Expo Go, `localhost` points to the phone itself, not your computer.
   - `fetch('http://localhost:3000/...')` will fail on a real device.

2. **API only bound to loopback (`127.0.0.1`)**
   - Even with your laptop IP in the URL, requests fail if the backend is not listening on all interfaces.

3. **Proxy environment variables breaking Node tooling**
   - `HTTP_PROXY` / `HTTPS_PROXY` / `ALL_PROXY` can make Expo/Metro/npm fail in ways that look like fetch problems.

4. **Corporate/VPN network restrictions**
   - VPN, firewall, or SSL interception can block Metro or API traffic.

### Fast checks

Run these where you start Expo:

```bash
# check whether a proxy is configured in shell
env | grep -Ei 'http_proxy|https_proxy|all_proxy|no_proxy'

# check npm proxy config
npm config get proxy
npm config get https-proxy
```

### Fixes

1. **Do not use `localhost` from Expo Go**
   - Replace API base URL with your machine LAN IP, for example:
     - `http://192.168.1.42:3000`

2. **Ensure backend listens on all interfaces**
   - Start backend on `0.0.0.0` (framework-specific), not only `127.0.0.1`.

3. **Temporarily clear proxy vars and retry Expo**
   ```bash
   unset HTTP_PROXY HTTPS_PROXY ALL_PROXY
   unset http_proxy https_proxy all_proxy
   npx expo start --tunnel --clear
   ```

4. **If npm proxy is set but not needed, clear it**
   ```bash
   npm config delete proxy
   npm config delete https-proxy
   ```

5. **Use tunnel mode when network routing is uncertain**
   ```bash
   npx expo start --tunnel
   ```

### Quick sanity endpoint test from your phone

Open phone browser and try:

```text
http://<YOUR-LAPTOP-IP>:<API-PORT>/health
```

If that URL does not load on the phone, Expo Go fetch requests to that API will fail too.
