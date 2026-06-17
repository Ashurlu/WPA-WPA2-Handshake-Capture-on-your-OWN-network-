# 🔌 Monitor Mode & Adapter Setup

This component covers passing a USB Wi-Fi adapter into Kali Linux running inside VMware, then enabling monitor mode — a prerequisite for all wireless capture work.

---

## What This Step Does

A monitor-mode capable adapter is required to see all 802.11 frames on a channel, not just frames addressed to the host machine. Because Kali runs in VMware, USB passthrough hands the physical hardware directly to the VM.

---

## VMware USB Passthrough

In VMware, go to **VM → Removable Devices** and connect the USB adapter to the virtual machine. Once attached, it appears as a network interface inside Kali.

Verify the adapter is visible:

```bash
iwconfig
```

Look for a `wlan0` (or similar) interface. If it shows `IEEE 802.11` you are ready to proceed.

---

## Enable Monitor Mode

```bash
sudo airmon-ng check kill   # kill processes that may interfere
sudo airmon-ng start wlan0  # place adapter in monitor mode
```

This creates a new interface — typically `wlan0mon` — that captures all frames on the current channel regardless of destination MAC address.

Confirm:

```bash
iwconfig wlan0mon
```

The `Mode` field should read `Monitor`.

---

## Notes

- Some adapters require firmware packages not installed by default on Kali; the separate [WiFi Adapter Setup (Alfa AWUS036ACH + VMware).md](WiFi%20Adapter%20Setup%20%28Alfa%20AWUS036ACH%20%2B%20VMware%29.md) covers the Alfa AWUS036ACH specifically.
- After finishing the lab, restore managed mode with `sudo airmon-ng stop wlan0mon`.
