# Implementing Snort IDS with Port Mirroring (SPAN)

![image alt](https://raw.githubusercontent.com/DEENSec/Snort-IDS-with-SPAN-Lab-Setup-and-Demo/refs/heads/main/01.png)

## Overview

This project provides a practical guide to deploying Snort, a powerful open-source Intrusion Detection System (IDS), integrated with port mirroring (Switched Port Analyzer, or SPAN) to enhance network security. By monitoring network traffic for malicious activities, the setup demonstrates how to detect threats such as ICMP pings, SSH brute-force attempts, and potential malware or DDoS attacks. The lab environment simulates a real-world network, using Snort to analyze traffic and generate alerts, enabling administrators to respond to potential security incidents effectively. The project emphasizes hands-on configuration, rule creation, and traffic analysis, making it suitable for both small and enterprise-level networks.
For a comprehensive walkthrough, including detailed setup instructions and screenshots, refer to the full article: Implementing Snort IDS with Port Mirroring (SPAN) in Your Network.


## Project Objectives

### Deploy Snort IDS

Install and configure Snort on an Ubuntu VM to monitor network traffic and detect suspicious activities.
Implement Port Mirroring: Configure SPAN on a Cisco switch to mirror traffic from a Windows client to the Snort machine for analysis.
Simulate Attacks: Use a Kali Linux VM to launch attacks (e.g., ping, SSH brute-force) to test Snort’s detection capabilities.
Create Custom Rules: Develop Snort rules to detect specific threats, such as ICMP traffic, SSH login attempts, and potential malware or DDoS activities.
Enhance Network Visibility: Demonstrate how SPAN and Snort together provide comprehensive monitoring of network traffic, including traffic targeting other devices.

## Lab Setup and Architecture
The lab consists of four key components, interconnected to simulate a realistic network environment:

### Ubuntu VM (Snort IDS)

Role: Hosts Snort for intrusion detection and traffic analysis.
Configuration: Snort is installed with promiscuous mode enabled on the network interface to capture all traffic. Custom rules are defined to detect specific threats.


### Windows Client VM

Role: Simulates an endpoint in the network.
Purpose: Receives traffic (e.g., pings, SSH attempts) that is mirrored to Snort for analysis.


### Kali Linux VM (Attack Machine)

Role: Simulates attacker behavior.
Tools: Uses Nmap for network scanning and tools like ping or SSH brute-force scripts to generate malicious traffic.


### Cisco Switch
Role: Facilitates network connectivity and port mirroring.
Configuration: SPAN is configured to mirror traffic from the Windows client’s port to the Snort machine’s port.



## Network Configuration

The Cisco switch routes traffic between the Windows client, Kali Linux VM, and Ubuntu VM.
SPAN is set up to copy all traffic from the Windows client’s port (e.g., f1/15) to the Snort machine’s port (e.g., f1/14), ensuring Snort can analyze traffic without interfering with network operations.
The Ubuntu VM’s network interface is set to promiscuous mode to capture all packets, including those not directly addressed to it.

## How It Works

### Snort Installation and Configuration

Snort is installed on the Ubuntu VM using the command sudo apt update && sudo apt install snort -y.
The network interface (e.g., ens33) is set to promiscuous mode (sudo ip link set ens33 promisc on) to capture all traffic.
The Snort configuration file (/etc/snort/snort.conf) is updated with the network range (HOME_NET) to define the monitored network.
Custom rules are added to /etc/snort/rules/local.rules to detect specific threats, such as ICMP traffic or SSH login attempts.


### Port Mirroring (SPAN)

SPAN is configured on the Cisco switch using commands like monitor session 1 source interface f1/15 both and monitor session 1 destination interface f1/14.
This setup ensures that all traffic to and from the Windows client is mirrored to the Snort machine for analysis.


### Attack Simulation

The Kali Linux VM simulates attacks, such as pinging the Windows client or attempting SSH brute-force logins using Nmap or other tools.
These attacks generate traffic that is captured by Snort, either directly (if targeting the Ubuntu VM) or via SPAN (if targeting the Windows client).


### Threat Detection and Alerts

Snort runs in IDS mode (sudo snort -q -l /var/log/snort/ -i ens33 -A console -c /etc/snort/snort.conf), analyzing traffic and generating console alerts based on rule matches.
Example rules include detecting ICMP traffic (alert icmp any any -> $HOME_NET any), SSH login attempts (alert tcp any any -> $HOME_NET 22), and potential malware or DDoS patterns.
Alerts are logged to /var/log/snort/ and displayed on the console, providing details like source IP, attack


