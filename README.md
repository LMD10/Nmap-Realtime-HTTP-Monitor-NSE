# Realtime HTTP Monitoring with Nmap NSE
## Overview

This project demonstrates the development of a custom Nmap Scripting Engine (NSE) script written in Lua to perform real-time HTTP service monitoring.

Unlike traditional NSE scripts that provide a single snapshot of a service, this script repeatedly polls an HTTP endpoint and records:
* Timestamp of request
* HTTP status code
* Request latency
* Response length
This enables time-based behavioral analysis of web services.

## What is Nmap?
Nmap (Network Mapper) is an open-source network discovery and security auditing tool widely used by:
* Network administrators
* Security analysts
* Penetration testers
* Blue team professionals
It performs host discovery, port scanning, service detection, and OS fingerprinting.

## What is NSE?
The Nmap Scripting Engine (NSE) allows users to extend Nmap functionality using the Lua programming language.
NSE enables:
* Automated service interrogation
* Vulnerability detection
* Custom protocol interaction
* Security auditing
* Information gathering
This project focuses on a custom discovery-category script.

## Script Purpose
The http-realtime-status.nse script:
* Executes on open TCP ports
* Sends repeated HTTP GET requests
* Measures latency using nmap.clock_ms()
* Timestamps results using os.date()
* Outputs structured results via stdnse.format_output()

This allows detection of:
* Temporary outages
* Latency spikes
* Content changes

Intermittent failures

## NSE Libraries Used
* http – Handles HTTP requests and responses
* stdnse – Output formatting utilities
* nmap – Timing and scanning functions
* os – Timestamp generation
* string – Data formatting

## Script Logic Architecture
1️⃣ portrule

Defines when the script runs:
* Only on open TCP ports
* Improves efficiency and safety

2️⃣ action

Core polling logic:
* Reads script arguments
* Performs iterative HTTP requests
* Calculates latency
* Records structured output
* Handles non-responses gracefully

## Default Parameters
| Argument   | Default | Description                  |
| ---------- | ------- | ---------------------------- |
| `iter`     | 5       | Number of polling iterations |
| `interval` | 2       | Seconds between polls        |
| `path`     | `/`     | Target HTTP path             |

## Execution Example
```bash
nmap -p 80 --script ./scripts/http-realtime-status.nse \
--script-args http-realtime-status.iter=5,http-realtime-status.interval=2 \
target.com
```

## Sample Output
```bash
Realtime HTTP polling (path=/, iterations=5, interval=2s)

[2025-11-08 14:32:01] code=200, latency=0.145s, length=1256
[2025-11-08 14:32:03] code=200, latency=0.102s, length=1256
...
```

## Key Learning Outcomes
* Understanding NSE architecture (portrule vs action)
* Writing modular Lua scripts
* Implementing time-based polling logic
* Handling runtime errors safely
* Producing structured output for reporting

## Ethical Use
This script performs non-intrusive HTTP GET requests and is categorized under "discovery".

It should only be used against:
* Systems you own
* Authorized lab environments
* Targets with explicit permission
