# DNS Caching in an SDN Network

Speeds up DNS lookups in a software-defined network by caching DNS responses inside the **Ryu SDN controller**, tested on a virtual network built with **Mininet**.

## The Problem

Frequently visited domain names are resolved again and again. Each lookup travels to the DNS server, which adds delay and wastes network resources.

## The Solution

The Ryu controller intercepts DNS traffic. When a domain has been looked up before, the controller answers from its cache instead of forwarding the request to the DNS server.

**Goals**

- Reduce DNS resolution time for repeated queries
- Implement a DNS cache inside the SDN architecture
- Improve overall network performance and reliability

## Tech Stack

| Tool | Role |
|---|---|
| Ryu | Python-based SDN controller (hosts the DNS cache logic) |
| Mininet | Emulates the virtual network topology |
| BIND9 | DNS server |
| Ubuntu on VirtualBox | Development and test environment |
| Python | Topology and controller scripts |

## Architecture

```
Mininet hosts  -->  OpenFlow switch  -->  Ryu controller (DNS cache)  -->  BIND9 DNS server
                                               |
                                     cache hit: reply immediately
```

## How to Test

1. Start the Mininet topology script.
2. Start the Ryu controller script with DNS caching enabled.
3. Open a terminal on a Mininet host with `xterm`.
4. Run the same lookup several times and compare response times:

   ```bash
   dig example.com
   nslookup example.com
   ```

   The first query goes to the DNS server. Repeat queries are answered from the cache and return faster.

## Challenges

Version conflicts between Mininet, Ryu, and Eventlet (Ryu's concurrency library) were the biggest hurdle. Pin compatible versions before setting up the environment.

## Report

The full design, implementation, and results are in [`Project Report.pdf`](Project%20Report.pdf).
