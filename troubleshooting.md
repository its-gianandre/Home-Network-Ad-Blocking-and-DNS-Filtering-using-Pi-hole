# Troubleshooting

During the deployment of the Pi-hole DNS filtering system, several configuration issues were encountered involving DNS routing, DHCP configuration, router compatability, and network performance. The following section documents the problems and the steps taken to resolve them.

## Pi-hole Query Log Showing No DNS Requests

### Problem
After installation, the Pi-hole dashboard showed zero DNS queries, even though multiple devices were connected to the network.
This indicated that devices were not using the Pi-hole as their DNS server.

### Cause
The router was still distributing its own DNS servers through DHCP instead of the Pi-hole server.
In many home networks, the router automatically assigns:
- Google DNS (8.8.8.8)
- IPS DNS servers
As a result, client devices bypass the Pi-hole.

### Troubleshooting Steps
1. Verified Pi-hole service was running using the command:
pihole status

2. Checked Pi-hole IP address using the command:
ip a

3. Confirmed that devices were receiving DNS settings from the router
4. Inspected router DHCP configuration

### Solution
Configured the network so that Pi-hole becomes the primary DNS resolver.

Typically, you would change the router DNS settings to the Pi-hole IP address. However, my Verizon router had DNS configuration limitations. The interface did not clearly allow changing DNS settings for the network.
The available menus only allowed editing device-level DNS entries, not network-wide DNS distribution.
Some ISP-provided routers restrict DNS configuration or hide advance settings.

To work around this issue, I explored several router menus including:
- Network settings
- DNS server configuration
- DHCP settings
- Static device entries

None allowed changing the DNS server globally. 

In order to work around this, I had to disable DHCP on the router, and enable it on the Pi-hole. This setting was found under "IPv4 Address Distribution". 
Pi-hole then distributes:
- IP addresses
- DNS server information

This ensures all devices automatically use Pi-hole. 

## Pi-hole Dashboard Became Unreachable

### Problem
After restarting the router, the Pi-hole dashboard could no longer be accessed. I had initially restarted the router because after configuring the Pi-hole and changing the DHCP/DNS settings, devices still weren't sending DNS queries to Pi-hole, and the dashboard showed no query logs.
Restarting the router forces:
- DHCP leases to refresh
- Devices to obtain the new DNS server (Pi-hole)
- The network configuration to reset

Anyways, the expected address:
http://(my-pihole-ip)/admin
was not reachable

### Cause
The Raspberry Pi was assigned a new IP address by the router, which broke the static DNS configuration.

### Troubleshooting Steps
1. Checked network interfaces using the command: ip a
2. Looked for the new IP address assigned to the Pi.
3. Checked router device list for the Raspberry Pi.

### Solution
Configured a static IP addrss to ensure the Pi-hole server always uses the same address.

Typically you would change the static IP on the Raspberry Pi itself using the command:
sudo nano /etc/dhcpcd.conf

and configure the file like this: (example) 
interface eth0
static ip_address=192.168.1.25/24
static routers=192.168.1.1
static domain_name_servers=127.0.0.1

however, I used a different method, which was to bind the Pi's MAC address to a fixed IP in the router settings. This approach centralizes network management on the router and avoids modifying system-level network configuration files on the Pi itself.

## Ads Not Being Fully Blocked

### Problem
Some websites continued displaying advertisements despite Pi-hole filtering being active. 

### Cause
Many modern websites load ads through:
- First-party domains
- Encrypted DNS
- Content delivery networks (CDNs)

These methods bypass traditional DNS blocklists.

### Troubleshooting Steps:
Verified domain queries through the Pi-hole query log.

Example blocked domains included:
secure-us.vtwenty.com
self.events.data.microsoft.com
global.telemetry.insights.video.a2z.com

However, some ads requests were embedded within allowed domains. One example of this would be Youtube. Their video ads are unable to be blocked due to the ads being embedded in the domain. It is important to note that while Pi-hole can block a number of ads, it cannot 100% block every single ad that is encountered.

### Solution
Improved filtering strength by adding additional blocklists.

This is the default blocklist included in the original installation of the Pi-hole:
https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts

Here are the additional blocklists I added:
- https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/pro.mini.txt (For mobile devices)
- https://cdn.jsdelivr.net/gh/hagezi/dns-blocklists@latest/adblock/ultimate.mini.txt (For devices with less RAM)
- https://big.oisd.nl (Very large list that prioritizes functionality over blocking)

This not only increased the number of blocked domains significantly, but also blocked phishing, malvertising, malware, spyware, ransomeware, cryptojacking, and telemetry/analytics/tracking. 

## Lessons Learned
Through troubleshooting with the Pi-hole deployment, several important networking principles were reinforced:
- Router DHCP configuration heavily impacts the Pi-hole server
- Static IP addressing is essential for network infrastructure
- ISP routers sometimes restrict advanced configuration
