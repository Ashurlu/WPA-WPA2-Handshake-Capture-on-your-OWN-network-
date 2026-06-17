# 🛡️ Defensive Recommendations

This component summarises the defensive countermeasures that directly address each stage of the WPA2 handshake-capture attack chain documented in this lab.

---

## Attack Chain vs. Defence Map

```
Attacker Step                    Defence
─────────────────────────────────────────────────────────────────
1. Passive scan (airodump-ng)  → Reduce signal exposure (lower TX power,
                                  directional antennas, physical placement)

2. Targeted capture            → Hidden SSID (minor friction only — not
                                  a real defence against airodump-ng)

3. Deauth burst                → Enable PMF / 802.11w (management frame
                                  protection makes deauth forgery hard)

4. Handshake captured          → Nothing prevents capture once the client
                                  reconnects — prevention must come earlier

5. Offline dictionary attack   → Strong passphrase is the ONLY defence
                                  at this stage; protocol cannot help
```

---

## Recommendations

### 1. Use a Long, High-Entropy Passphrase

A random 16+ character passphrase (mixed case, digits, symbols) is not present in any public wordlist and is computationally impractical to brute force even with GPU acceleration.

**Avoid:**
- Numeric-only passwords (e.g., `987654321`)
- Dictionary words, names, dates
- Passwords shorter than 12 characters

### 2. Migrate to WPA3

WPA3-SAE (Simultaneous Authentication of Equals) replaces the pre-shared key exchange with a zero-knowledge proof. Even if the 4-way handshake is captured, no offline attack is possible against the SAE exchange.

### 3. Enable Protected Management Frames (PMF / 802.11w)

PMF cryptographically authenticates management frames, making it significantly harder to forge the deauthentication frames used in Step 3 of the attack. Most modern routers support this under "MFP" or "802.11w" settings — set it to **Required** rather than **Optional**.

### 4. Reduce Signal Exposure

Minimising the physical area where frames can be captured reduces the attacker's window:

- Lower router transmit power to cover only the intended area
- Use directional antennas to avoid broadcasting into adjacent spaces
- Place routers away from exterior walls and windows

### 5. Monitor for Deauth Floods

A burst of deauthentication frames is a strong signal that a handshake-capture attempt is in progress. Wireless intrusion detection systems (WIDS) — including open-source tools like **Kismet** — can alert on abnormal management frame rates.

---

## Summary

The WPA2 protocol itself is sound; it is the offline crackability of weak passphrases that makes this attack practical. A random, long passphrase closes the critical gap. PMF and WPA3 address the earlier stages of the chain.
