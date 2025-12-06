---
title: "FreeBSD init"
date: 2025-12-01T20:31:22+07:00
---


In last month, I began experimenting with FreeBSD after watching several of their engineering workgroup calls on YouTube. The team appeared genuinely committed to improving laptop support. This is a notoriously difficult challenge. Despite decades of effort, Linux has still not achieved the long-predicted “Year of the Linux Desktop” (a meme that resurfaces predictably every year, even after more than 15 years of personal use on my part). The closest the Linux desktop market share has ever come to reaching 5 % in recent times has been driven by the Steam Deck and, ironically, by Microsoft.

FreeBSD is a proven OS on the server, its running busiest server reliably. It is a thoughtfully designed and cohesive OS, with careful engineering from the kernel all the way to userspace. In contrast to its counterpart Linux, where the kernel is excellent but userspace tends to be fragmented and inconsistent due to each distribution following its own conventions.

My initial project with FreeBSD is to move off my Tailscale's exit node to WireGuard to act as a VPN. I tried to replace my cheap VPS from Digital Ocean to Contabo, as they offer FreeBSD 14.0 as a base installer.

WireGuard is implemented as a native kernel module in FreeBSD, which generally offers excellent performance. Unfortunately, I initially spent several hours attempting to set up my VPN server without success. I could ping the server successfully, but all internet connectivity would drop the moment the tunnel was activated.

The root cause was that many budget VPS providers (such as Contabo, Hetzner Cloud, etc) do not properly expose the necessary capabilities for WireGuard’s kernel module to function correctly under KVM. As a result, packets from WireGuard clients are silently dropped at the host level.

The reliable workaround is to run WireGuard in userspace mode using the wireguard-go and wg-quick userspace tools. The setup is slightly more manual compared to solutions like Tailscale, but it is straightforward and gives you complete control over the configuration.

Note: At the time of writing, the latest FreeBSD release is 15.0-RELEASE. Many low-cost VPS providers still deploy images based on older branches (often 14.0 or earlier). Attempting to upgrade directly from the provider’s base image to 15.0 will almost certainly fail due to ABI changes and missing release artifacts. The safe upgrade path is to first upgrade to the latest 14.3-RELEASE, then proceed to 15.0 once the system is fully updated and stable.

The following is a complete, step-by-step guide to setting up a personal WireGuard VPN server on FreeBSD using the userspace implementation, tested on Contabo ($5/mo) and similar budget KVM hosts.


```sh
# 1. Update the package repository for the latest available packages
pkg update

# 2. Install the userspace WireGuard implementation (this includes wireguard-go and tools)
pkg install wireguard-go wireguard-tools

# 3. Verify the installation
pkg info | grep wireguard

# 4. Create the WireGuard directory if it does not exist
mkdir -p /usr/local/etc/wireguard

# 5. Generate server private/public key pair (ensure umask for security)
umask 077
wg genkey | tee /usr/local/etc/wireguard/server-private.key | wg pubkey > /usr/local/etc/wireguard/server-public.key

# 6. generate keys for your devices
wg genkey | tee /usr/local/etc/wireguard/mbp-private.key | wg pubkey > /usr/local/etc/wireguard/mbp-public.key

# 7. Create the server configuration file (you may need to install vim)
vim /usr/local/etc/wireguard/wg0.conf
```

Insert the following content:

```sh
[Interface]
PrivateKey = <contents of /usr/local/etc/wireguard/server-private.key>
Address = 10.77.0.1/24
ListenPort = 51820

# Example peer (client device)
[Peer]
PublicKey = <client-public-key>
AllowedIPs = 10.77.0.2/32
```

create a new file for client config (e.g touch mbp_client.conf)
```sh
# 8. create client config for each devices to want to connect
[Interface]
PrivateKey = <paste your client-private.key here>
Address = 10.77.0.2/32          # any address in the same subnet, .2 is fine
DNS = 1.1.1.1, 1.0.0.1          # optional but recommended

[Peer]
PublicKey = <paste the SERVER public key here>
# → cat /usr/local/etc/wireguard/server-public.key on the VPS and copy it

AllowedIPs = 0.0.0.0/0         # route ALL traffic through the VPS
# AllowedIPs = 10.77.0.0/24    # use this instead if you only want LAN access, not full tunnel
Endpoint = YOUR.VPS.IP.OR.HOSTNAME:51820
PersistentKeepalive = 30

```

Save and exit. the use the following command

```sh
# 9. Enable and start the WireGuard service (userspace mode activates automatically)
#sysrc wireguard_enable="YES"
#sysrc wireguard_interfaces="wg0"
#service wireguard start
wg-quick up wg0
```

Check your interface using ifconfig command and set the following PF rule

```sh
# 10. Set PF (packet Filter) rule /etc/pf.conf
match out on eth0 from wg0:network to any nat-to (eth0)

pass in on wg0
pass out on eth0

pass in quick proto udp from any to eth0 port 51820 keep state
```


Install wireguard to each your device then transfer client configuration (eg. mbp_client.conf) to each device and load to wireguard.

Verify your packets received by the VPN server

```sh
# 11. Verify the interface is up
ifconfig wg0
wg show wg0
```

PS: updated /etc/pf.conf rules to a stricter one

```sh
ext_if = "eth0"
wg0_if = "wg0"
wg0_networks = "10.77.0.0/24"

set skip on lo

nat on $ext_if from { $wg0_networks } to any -> ($ext_if)

pass quick on $wg0_if

block in
block out

# Allow SSH (change port if you moved it)
pass in on $ext_if proto tcp to port ssh
# WireGuard itself
pass in on $ext_if proto udp to port 51820

pass out on $ext_if
```

