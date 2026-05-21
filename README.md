## WPA2 Handshake Capture & Password Cracking Lab

**Educational Purpose Only — This lab was performed on a personally owned hotspot for learning wireless security concepts**  

## Demonstrate the WPA2 4-way handshake capture process on a personal hotspot and attempt an offline dictionary attack using aircrack-ng. The goal is to understand:

 - How WPA2 authentication works  
 - Why weak passwords are a critical vulnerability  
 - How attackers exploit offline cracking after minimal on-site exposure

# ⚖️ Legal Disclaimer

⚠️ All tests were performed exclusively on a personally owned Wi-Fi hotspot.  
Capturing handshakes or attempting to crack passwords on networks you do not own is illegal under computer fraud laws in most countries.  
This project is strictly for educational and academic purposes.  

# 🛠️ Tools Used
Kali Linux -> Penetration testing OS  
airmon-ng -> Enable/disable monitor mode on wireless adapter  
airodump-ng -> Capture wireless packets and handshakes  
aireplay-ng -> Send deauthentication frames to force reconnection  
aircrack-ng -> Dictionary-based WPA2 password cracking  
hashcat (optional) -> GPU-accelerated password cracking  
hcxpcapngtool (optional) -> Convert .cap to hashcat-compatible format  
rockyou.txt -> Common password wordlist (14+ million entries)  

# 💻 Environment Setup
 - OS: Kali Linux (latest rolling release)
 - Target: Personal iPhone 15 hotspot (owned device)
 - Wireless Adapter: Monitor-mode capable adapter (Mine Alfa network, network adapter
 - Network: Isolated personal hotspot — no third-party networks involved
   

