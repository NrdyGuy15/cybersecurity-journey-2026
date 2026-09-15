# Lab 01 — VM Network Connectivity

**Goal:** Get two VMs (Kali and Metasploitable2) onto the same isolated network and prove they can actually communicate.

## Setup
- **Kali Linux** — attacker box
- **Metasploitable2** — intentionally vulnerable Linux target
- Both running in Oracle VirtualBox

## What I did
1. Booted both VMs and pulled each machine's IP address (`ip a` on Kali, `ifconfig` on Metasploitable2).
2. Pinged Metasploitable2 from Kali and got a "4/4 received" success — but it was a **false positive**. The two machines were on *different* VirtualBox networks (Kali on NAT Network, Metasploitable2 on Host-Only), so that reply wasn't really coming from the target.
3. Diagnosed it from the addresses: Kali was `10.0.2.x` (NAT Network), Metasploitable2 was `192.168.56.x` (Host-Only) — two separate networks that can't talk.
4. Changed Metasploitable2's Adapter 1 from **Host-Only** to **NAT Network** in VirtualBox settings.
5. Ran `sudo dhclient eth0` on Metasploitable2 to release its stale address and pull a fresh one on the correct network.
6. Confirmed both machines were now on the same `10.0.2.0/24` network — Kali `10.0.2.3`, Metasploitable2 `10.0.2.5`.

## Commands used
| Command | What it does |
|---------|--------------|
| `ip a` | List network interfaces and IP addresses (modern Linux) |
| `ifconfig` | Same, on older systems like Metasploitable2 |
| `ping -c 4 <ip>` | Send 4 test packets to a host and check for replies |
| `sudo dhclient eth0` | As root, request a fresh DHCP-assigned IP on the eth0 interface |

## What I learned
- A successful ping isn't proof of anything until you know *what* answered — `127.0.0.1` is loopback (a machine talking to itself), not a real target.
- Two VMs can only communicate if they're on the **same** VirtualBox network. NAT Network lets them see each other *and* reach the internet, while staying isolated from my home network.
- Moving a VM to a new network doesn't update its IP automatically — you have to renew the DHCP lease.

## Still to do
- Reverse ping (Metasploitable2 → Kali) to confirm two-way communication.
