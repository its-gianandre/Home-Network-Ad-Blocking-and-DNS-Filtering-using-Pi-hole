# Pi-hole Setup Guide

## Requirements:
- Raspberry Pi (You can use the Zero W model and up, but I used the Pi 4 Model B)
- Raspberry Pi OS
- Ethernet connection (preferably)
- Router access
- Micro SD card (32 GB or more)

## Step 1 - Install Raspberry Pi OS
Flash Raspberry Pi OS to the micro SD card using Raspberry Pi Imager.

## Step 2 - Update the system
sudo apt update && sudo apt upgrade -y

## Step 3 - Install Pi-hole
curl -sSL https://install.pi-hole.net | bash

## Step 4 - Configure Static IP
There are 2 different methods that you can use to go about this step

#### First method: Edit dhcpcd.conf (most common method)
(I did not use this method when configuring my Pi)

Edit the DHCP configuration file: 
sudo nano /etc/dhcpcd.conf 

Add something like this at the bottom: 
interface eth0
static ip_address=192.168.1.25/24
static routers=192.168.1.1
static domain_name_servers=127.0.0.1

Then reboot: 
sudo reboot

#### Second Method: Router DHCP Reservation
(This is the method I used)

Instead of configuring the Pi itself, I configured the router to always give the Pi the same IP address.

Steps:
1. Log into your router
2. Go to DHCP settings
3. Find DHCP reservation/static lease
4. Select the Pi's MAC address
5. Assign a fixed IP

## Step 5 - Router Configuration
Set the router DNS server to the Pi-hole IP address

## Step 6 - Access Dashboard
http://(pihole-ip)/admin
or if you're on the raspberry pi itself, you can also use
http://localhost/admin
