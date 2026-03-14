# Home-Network-Ad-Blocking-and-DNS-Filtering-using-Pi-hole
This project implements a network-wide DNS filtering solution using Pi-hole on a Raspberry Pi. The goal was to block ads, trackers, and malicious domains across all devices on a home network while improving privacy and network security.

### Key Features:
- Network-wide ad blocking
- DNS filtering
- Malware domain blocking
- Centralized monitoring dashboard
- Custom blocklists

### Objectives of this project:
- Learn how DNS filtering works
- Implement network-level ad blocking
- Improve home network privacy
- Gain experience with Raspberry Pi and Linux
- Monitor DNS traffic and block malicious domains

### Tools and technologies used:
- Raspberry Pi 4 Model B: Used to host the Pi-hole server
- Pi-hole: DNS filtering and ad blocking
- Linux (Raspberry Pi OS)
- Verizon Router: Network gateway
- Blocklists: Domain filtering

### Network Architecture:
The Raspberry Pi running Pi-hole acts as the network's DNS server.
All devices send DNS queries to Pi-hole instead of external DNS providers.

### Process: 
1. Device requests a website
2. DNS query sent to Pi-hole
3. Pi-hole checks blocklists
4. If domain is blocked -> request denied
5. If allowed -> forwarded to upstream DNS (specifically Cloudflare DNSSEC)
6. IP address returned to device

For diagram of the process see:
diagrams -> network_architecture.png

### Setup Instructions:
For full installation instructions see:
setup-guide.md


