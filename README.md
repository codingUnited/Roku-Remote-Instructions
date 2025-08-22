# Roku-Remote-Instructions

# 📱 Roku Remote Project — Setup & First Demo

## 1. Verify Roku Can Receive Commands

### Find Your Roku IP
- On Roku: `Settings → Network → About → note IP Address`
- Example IP: `10.0.0.154`

### Check Device Info
```bash
curl -v http://<ROKU_IP>:8060/query/device-info
```

If `curl` doesn’t work (Windows PowerShell):
```powershell
Invoke-WebRequest -Uri "http://<ROKU_IP>:8060/query/device-info" -Method GET -Verbose
```

✅ Confirm:
- Response status is `200`
- XML is returned
- `<ecp-setup-mode>` is `default` or `permissive`

### If Not Permissive
On Roku:
- `Settings → System → Advanced System Settings → Control by Mobile Apps / External Control → Network Access`
- Set to **Permissive**

### Send Test Input
```bash
curl -v -d '' http://<ROKU_IP>:8060/keypress/Home
```

If `curl` doesn’t work (Windows PowerShell):
```powershell
Invoke-WebRequest -Uri "http://<ROKU_IP>:8060/keypress/Home" -Method POST
```

➡ Roku should go back to its **Home screen**.

> **Note:** On Roku TVs you must set network access to *Permissive*. Some Roku TVs won’t show the full XML return.

---

## 2. Windows Setup

### Install Node.js (LTS)
- Download from [nodejs.org](https://nodejs.org)
- Run installer → accept defaults (includes `npm`)

### Verify Installation
```bash
node -v
npm -v
```

If you get a script execution error:
1. Close PowerShell
2. Open PowerShell as Administrator
3. Run:
```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```
Enter `Y` when prompted.

---

## 3. Create Expo App

```bash
npx create-expo-app roku-remote --template blank
cd roku-remote
```

### Start Dev Server
```bash
npx expo start
```

- Browser will open with a QR code
- Open **Expo Go** on your phone → scan QR

---

## 4. Add Roku Command Button

Edit `App.js` — delete all code and paste:

```javascript
import { View, Button } from "react-native";

export default function App() {
  const rokuIp = "10.0.0.154"; // Replace with your Roku IP

  function goHome() {
    fetch(`http://${rokuIp}:8060/keypress/Home`, { method: "POST" });
  }

  return (
    <View style={{ flex: 1, justifyContent: "center" }}>
      <Button title="Go Home" onPress={goHome} />
    </View>
  );
}
```

➡ Press the button → Roku should return to **Home**.

---
