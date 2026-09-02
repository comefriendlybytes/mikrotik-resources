
# MikroTik RouterOS `grc` Log Colorizer

![GRC Required](https://img.shields.io/badge/Dependency-GRC-orange.svg)
![RouterOS Compatibility](https://img.shields.io/badge/RouterOS-v6%20%7C%20v7-blueviolet.svg)  
![Syslog Compatible](https://img.shields.io/badge/Syslog-Compatible-00599C?style=flat&logo=syslog-ng)

A custom **Generic Colorizer (`grc`)** configuration file (`conf.mikrotik`) for colorizing live MikroTik RouterOS syslog streams and log files in the terminal.

It highlights hostnames, IP/MAC addresses, severe actions, wifi bands (`wifi1` vs `wifi2`), and dynamically color-codes wireless signal strength ranges ($dBm$) from strong to poor.

---
![screenshot](https://raw.githubusercontent.com/comefriendlybytes/mikrotik-resources/main/grc_conf/screenshot.png)

---

## Quick Setup

Copy the following commands to your terminal to setup the configuration files.

### 1. Append Mapping Location to `/etc/grc.conf`

  

    # MikroTik logs

        \bmikrotik.*\.log\b

        conf.mikrotik

### 2. Create Mapping /usr/share/grc/[conf.mikrotik](conf.mikrotik)
    
      

    
    # MikroTik RouterOS Log Configuration for GRC


    # Radio 1 (2.4 GHz / wifi1)

    regexp=(@?wifi1)\b

    colors=bold cyan

    -

    # Radio 2 (5 GHz / wifi2)

    regexp=(@?wifi2)\b

    colors=bold magenta

    -

    # Wireless Signal Strength - Good (-30 to -67)

    regexp=\bsignal strength -(?:[3-5]\d|6[0-7])\b

    colors=bold green

    -

    # Wireless Signal Strength - Fair (-68 to -75)

    regexp=\bsignal strength -(?:6[89]|7[0-5])\b

    colors=yellow

    -

    # Wireless Signal Strength - Poor (-76 to -99)

    regexp=\bsignal strength -(?:7[6-9]|[89]\d)\b

    colors=bold red

    -

    # Timestamps (e.g., jan/01/2026 12:00:00 or ISO formats)

    regexp=\b\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}[+-]\d{2}:\d{2}\b|\b([a-z]{3}/\d{2}(/\d{4})?\s+)?\d{2}:\d{2}:\d{2}\b

    colors=magenta

    -

    # System identity / Hostname

    regexp=^(\S+)\s+

    colors=bold cyan

    -

    # Severe Errors & Failures

    regexp=\b(error|critical|failed|failure|down|denied|unreachable|link down|disconnected|connection lost)\b

    colors=bold red

    -

    # Warnings & Suspicious Activity

    regexp=\b(warning|warn|dropped|timeout|link up|reboot|rebooted|roamed|deassigned)\b

    colors=yellow

    -

    # Success & Connection States

    regexp=\b(info|connected|established|assigned|login|logged in)\b

    colors=green

    -

    # IP Addresses (IPv4)

    regexp=\b(?:[0-9]{1,3}\.){3}[0-9]{1,3}(/\d{1,2})?\b

    colors=bright_cyan

    -

    # MAC Addresses

    regexp=\b([0-9A-Fa-f]{2}[:-]){5}([0-9A-Fa-f]{2})\b

    colors=bright_magenta

    -

    # MikroTik Topics & Protocols (e.g., system,info, dhcp,critical, firewall,info)

    regexp=\b(system|dhcp|firewall|interface|wireless|dns|route|ipsec|ppp|pppoe|caps|script|account)\b

    colors=cyan

    -

    # Interface Names (e.g., ether1, wlan1, sfp-sfpplus1, bridge1, vlan10)

    regexp=\b(ether\d+|wlan\d+|sfp\d*|bridge\d*|vlan\d+|bond\d+|loopback\d*)\b

    colors=bold blue

    -

    # Users / Logins

    regexp=\b(user\s+\w+|by\s+\w+)\b

    colors=bold yellow

    -

    # Firewall Actions

    regexp=\b(action=drop|chain=input|chain=forward|drop|denied|reject)\b

    colors=bold red

    -


    regexp=\b(action=accept|accept)\b

    colors=bold green

    -


    regexp=\b(action=passthrough|action=log)\b

    colors=bold yellow

    -

    # Bandwidth Rates (e.g., 10Mbps, 100Mbps, 1Gbps, 2.5Gbps+)

    regexp=\b\d+(\.\d+)?\s*(Kbps|Mbps|Gbps|kbps|mbps|gbps)\b

    colors=bright_blue

    -
    
    
# Testing & Usage

## Test via Terminal Echo

	echo "2026-09-01T16:50:56+01:00 xxxx-router wireless,info AE:B9:B1@wifi1(MikroTik-) roamed to AE:B9:B1@wifi2(MikroTik-), signal strength -76" | grc -c conf.mikrotik cat


# Tail Live Logs

> [!NOTE]
> **Piped Input vs. Log Files:** The regular expression `\bmikrotik.*\.log\b` in `/etc/grc.conf` only runs when `grc` is called directly against a file matching that pattern (e.g., `grc tail -f /var/log/mikrotik.log`). 
> 
> When piping arbitrary streams (e.g., `tail -f /var/log/syslog | grc cat`), `grc` does not know the context of the stream. In those cases, pass the configuration file explicitly using the `-c` flag:
> ```bash
> tail -f /var/log/syslog | grc -c conf.mikrotik cat
> ```

## Direct log file

    grc -c conf.mikrotik tail -f /var/log/mikrotik.log

## Streamed via Syslog

	tail -f /var/log/syslog | grc -c conf.mikrotik cat

# Shell Alias

    # Add to ~/.bashrc or ~/.zshrc
    alias mtail='grc -c conf.mikrotik tail -f'

### Topics & Tags

`mikrotik` • `routeros` `grc` • `generic-colorizer` • `syslog` • `log-colorizer` • `wifi` • `terminal-colors` • `network-monitoring` • `bash`
