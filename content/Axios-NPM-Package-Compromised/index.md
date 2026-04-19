---
title: Axios NPM Package Compromised 
description: 
draft: false
tags:
modified: 2026-03-31
created: 2026-03-31
---

# Axios NPM Package Compromised: Supply Chain Attack Hits JavaScript HTTP Client with 100M+ Weekly Downloads

In a sobering reminder of just how fragile our software supply chains can be, the hugely popular JavaScript library Axios was hit by a sophisticated supply chain attack in late March 2026. With over 100 million weekly downloads on npm, Axios has been a trusted favourite for developers who need to make HTTP requests in both the browser and Node.js. Its massive popularity made this breach especially worrying, potentially putting thousands of projects and live systems at risk.

### The Anatomy of the Attack

On 31 March 2026, between roughly 00:21 and 03:20 UTC, two malicious versions of the package were quietly published to the npm registry. These were axios@1.14.1 (tagged as "latest") and axios@0.30.4 (tagged as "legacy").

The attacker managed to compromise the npm account of the lead maintainer, jasonsaayman. They quickly changed the registered email address to an attacker-controlled ProtonMail account (ifstap@proton.me). This effectively locked out the real owner and blocked any quick recovery.

Importantly, these compromised versions did not come from the official Axios GitHub repository. There were no matching commits, tags, or signs of activity in the CI/CD pipeline. The attacker simply pushed the updates directly using the npm CLI.

The trick involved a hidden dependency called plain-crypto-js@4.2.1. This package had been prepared the day before with what looked like a harmless version. It was never actually used in Axios code. Its only job was to run a postinstall script as soon as the package was installed.

Once triggered, the script acted as a stealthy dropper. It reached out to a command-and-control server, downloaded a cross-platform remote access trojan (RAT), and installed it on the machine. The malware worked on Windows, macOS, and Linux, and included features to help it stay hidden and persistent. Some reports mentioned it even tried to cover its tracks by deleting parts of itself.

The malicious versions stayed live for only about two to three hours before npm took them down. Still, in that short time, many automated builds, CI pipelines, and developer machines probably pulled the bad package, especially because of the "latest" tag.

Security researchers from Elastic, Huntress, Google Threat Intelligence, Microsoft and others have linked the attack to a North Korean state-sponsored group. This actor is known by names such as Sapphire Sleet, UNC1069, and NICKEL GLADSTONE. They have a history of going after cryptocurrency projects, DeFi platforms, and valuable tech targets for both money and espionage.

### Why This Matters: The Perils of Dependency Hell

Because Axios is used in so many modern applications and cloud environments, the impact of this attack is significant. A single compromised maintainer account, without strong multi-factor protection or proper publishing safeguards, became the weak point that put millions of users at risk.

This incident clearly shows the uneven nature of supply chain attacks. Attackers only need to break one trusted piece to affect a huge number of people downstream.

The clever use of a postinstall script also highlights a long-standing weakness in the npm ecosystem. While these scripts are convenient, they can easily be abused to run harmful code during installation.

### Lessons and Immediate Actions for Developers

If your project installed axios@1.14.1 or axios@0.30.4 during that window, you should treat the system as potentially compromised. Here are the key steps to take right away:

- Check your lockfiles (package-lock.json, yarn.lock, or pnpm-lock.yaml) and scan your dependency tree for the malicious versions.
- Switch back to a clean, verified version such as 1.15.0 and do a fresh install.
- Scan your machines for signs of infection, especially any suspicious network connections.
- Tighten your npm settings by enabling ignore-scripts, setting minimum release ages, and considering safer package managers like pnpm or Bun.
- Start using better supply chain practices such as provenance attestations, multi-person approval for publishing, and regular dependency security scans.

For widely used packages like Axios, the community needs to push for stronger protections. This includes mandatory hardware-backed multi-factor authentication, better anomaly detection when new versions are published, and moving away from relying on single maintainer accounts.

This attack is yet another clear warning about why we must stay vigilant with open-source dependencies. In today's world, where our digital infrastructure depends so heavily on these shared packages, even a short security lapse can lead to serious consequences.

**AbhikrX**  
@importcookiesre  
Ranchi, March 2026