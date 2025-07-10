# INE Lab - Host Discovery & Service Enumeration with Nmap

## Objective
Identify active hosts and running services on `demo.ine.local` network, simulating an ICMP-blocked environment.

## Methodology

### 1. Initial Ping Test
```bash
ping -c 4 demo.ine.local
```
- **Results**: 
  - No host response (ICMP blocking detected).
  - ![Ping failure](Images/INE/ping_scan.png)  
  *Figure 1: Ping test failure due to ICMP filtering*

### 2. Basic Nmap Scan
```bash
nmap demo.ine.local
```
- **Results**: 
  - False negative (without `-Pn` flag).
  *Figure 2: Host still appears offline*

### 3. Host Status-Ignorant Scan (`-Pn`)
```bash
nmap -Pn demo.ine.local
```
- **Results**: 
  - 3 open ports (22/SSH, 80/HTTP, 443/HTTPS).
  - ![Pn Scan Results](Images/INE/nmap_pn_scan.png)  
  *Figure 3: -Pn scan revealing open ports*

### 4. Service Version Detection (`-sV`)
```bash
nmap -Pn -sV demo.ine.local
```
- **Results**: 
  - Identified services:
    - Apache 2.4.7 (CVE-2020-11984)
    - OpenSSH 7.4
  - ![Version detection](Images/INE/nmap_sv_results.png)  
  *Figure 4: Service versions detected*

## Key Findings
- ICMP blocking doesn't indicate host inactivity. Nmap's `-Pn` flag bypasses this limitation.
- Service enumeration (`-sV`) revealed vulnerable software versions.
- **Next Steps**: Test vulnerabilities using `--script vuln` or Metasploit modules.

## Useful Commands
```bash
# Quick scan for ICMP-restricted networks
nmap -Pn --top-ports 100 192.168.1.1

# Aggressive service detection
nmap -Pn -sV -sC -O demo.ine.local
```

## References
- [Nmap Official Documentation](https://nmap.org/book/man.html)
- [MITRE ATT&CK: Discovery Techniques](https://attack.mitre.org/tactics/TA0007/)

---
> **Pro Tip**: Add this badge to your README:  
> `[![Nmap](https://img.shields.io/badge/Nmap-5.0%2B-brightgreen)](https://nmap.org)`
