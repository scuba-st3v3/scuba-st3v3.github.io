---
layout: post
title:  "My Home Network Setup"
date:   2026-04-01 10:00:00 -0500
categories: homelab
permalink: /:categories/:title/
---

I've been running a home network that has evolved over the years into a setup I'm pretty happy with. This post is a living reference for my own use, but I hope it helps anyone else thinking about building something similar.

## Network Architecture

At a high level, traffic flows like this:

**Internet → OPNsense Firewall → UniFi Pro 24 Switch → APs / Servers / Devices**

I also run a separate VLAN and WiFi network for IoT devices to keep them isolated from the main network.

## Firewall: OPNsense on Protecli FW4B

**Hardware:** [Protecli FW4B](https://protectli.com/vault-4-port/)

**Role:** Main router, firewall, intrusion detection, and DNS filtering

I originally built this firewall in 2020 running pfSense. In 2025, the M.2 SSD failed and I took the opportunity to migrate to [OPNsense](https://opnsense.org/). I've been happy with the switch — the interface feels cleaner and the update process has been smooth.

**Key services running:**

- **Suricata** — Intrusion detection
- **Unbound DNS** — With blocklists for ads, trackers, and malware:
  - AdGuard DNS Filter
  - Hagezi Normal
  - Hagezi Threat Intelligence Feed
  - Steven Black List
- **Monit** — Monitors gateway latency and alerts if the connection goes down
- **Cron jobs** — Automated system updates, IDS rule updates, and DNS blacklist updates

## Switch: UniFi Pro 24 (USW-Pro-24)

**Hardware:** [UniFi Pro 24](https://store.ui.com/us/en/products/usw-pro-24)

**Role:** Core managed switch

The USW-Pro-24 handles all wired connections. It provides PoE (via power injector) for the access points and is managed through the UniFi Controller.

## Wireless Access Points

**Hardware:**

- [UniFi AP AC Pro](https://store.ui.com/us/en/products/uap-ac-pro)
- [UniFi AP AC Lite](https://store.ui.com/us/en/products/uap-ac-lite)
- [U6 Extender](https://store.ui.com/us/en/products/u6-extender)

All three are managed through the UniFi Controller. They provide whole-home coverage and support multiple SSIDs, including the isolated IoT network.

## Network Segmentation: IoT VLAN

I run a separate WiFi network on its own VLAN for IoT devices. The goal is simple: if a cheap smart plug or camera gets compromised, it shouldn't have access to the rest of the network. The firewall rules restrict IoT traffic so devices can reach the internet but not the main LAN.

## Storage Server: TrueNAS

**Hardware:** 16 GB ECC RAM, RAIDZ1 pool (3x 4TB drives)

**Role:** Network-attached storage

**Other services running:**

- **Plex Server**
- **UPS** - CyberPower BRG1350AVRLCD
- **Photo Backup** - Cloud Sync to Backblaze
- **Time Machine** - Backups for Apple devices

I originally built this TrueNAS (FreeNAS) box in 2018. It does require a lot of care and feeding to keep things up-to-date. In 2025, I started having system stability issues and ended up replacing both the power supply and the RAM. The RAIDZ1 pool gives me a good balance of capacity and redundancy for home use.

---

I'll likely write follow-up posts going deeper into specific components or configurations.
