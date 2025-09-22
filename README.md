# Nmap
A comprehensive guide/cheatsheet for learning Nmap
# Nmap Comprehensive Learning Guide & Cheatsheet

## Table of Contents
- [Introduction](#introduction)
- [Installation](#installation)
- [Basic Concepts](#basic-concepts)
- [Basic Scanning Techniques](#basic-scanning-techniques)
- [Port Scanning Options](#port-scanning-options)
- [Host Discovery](#host-discovery)
- [Service Detection](#service-detection)
- [OS Detection](#os-detection)
- [Scripting Engine (NSE)](#scripting-engine-nse)
- [Output Formats](#output-formats)
- [Timing and Performance](#timing-and-performance)
- [Firewall Evasion](#firewall-evasion)
- [Advanced Techniques](#advanced-techniques)
- [Common Use Cases](#common-use-cases)
- [Best Practices](#best-practices)
- [Legal and Ethical Considerations](#legal-and-ethical-considerations)

## Introduction

Nmap (Network Mapper) is a free and open-source network discovery and security auditing tool. It's used for network inventory, managing service upgrade schedules, and monitoring host or service uptime.

### What Nmap Can Do
- Host discovery
- Port scanning
- Service version detection
- Operating system detection
- Vulnerability detection
- Network topology mapping
- Firewall rule testing

## Installation

### Linux (Debian/Ubuntu)
```bash
sudo apt update
sudo apt install nmap
```

### Linux (Red Hat/CentOS)
```bash
sudo yum install nmap
# or for newer versions
sudo dnf install nmap
```

### macOS
```bash
# Using Homebrew
brew install nmap

# Using MacPorts
sudo port install nmap
```

### Windows
Download from official website: https://nmap.org/download.html

## Basic Concepts

### Target Specification
```bash
# Single IP
nmap 192.168.1.1

# IP range
nmap 192.168.1.1-254

# CIDR notation
nmap 192.168.1.0/24

# Multiple targets
nmap 192.168.1.1 192.168.1.5 192.168.1.100

# Hostname
nmap scanme.nmap.org

# Read targets from file
nmap -iL targets.txt

# Exclude targets
nmap 192.168.1.0/24 --exclude 192.168.1.1
nmap 192.168.1.0/24 --excludefile exclude.txt
```

### Port Specification
```bash
# Single port
nmap -p 22 target

# Multiple ports
nmap -p 22,80,443 target

# Port range
nmap -p 1-1000 target

# All ports
nmap -p- target

# Most common ports
nmap --top-ports 1000 target

# Specific protocols
nmap -p U:53,T:80 target  # UDP port 53, TCP port 80
```

## Basic Scanning Techniques

### Ping Scan (Host Discovery Only)
```bash
# Basic ping scan
nmap -sn 192.168.1.0/24

# Also called "ping sweep"
nmap -sP 192.168.1.0/24  # Deprecated, use -sn
```

### TCP Connect Scan
```bash
# Default scan type for non-privileged users
nmap -sT target

# Most reliable but easily detected
```

### SYN Scan (Stealth Scan)
```bash
# Default scan for privileged users
nmap -sS target

# Half-open scan, harder to detect
```

### UDP Scan
```bash
# Scan UDP ports
nmap -sU target

# Combine with TCP scan
nmap -sS -sU target
```

## Port Scanning Options

### Comprehensive Port Scans
```bash
# Fast scan (100 most common ports)
nmap -F target

# Intense scan
nmap -T4 -A -v target

# Aggressive scan
nmap -A target

# Version detection
nmap -sV target

# OS detection
nmap -O target

# Script scan
nmap -sC target

# All-in-one aggressive
nmap -A -T4 target
```

### Specific Scan Types
```bash
# TCP ACK scan (firewall rule testing)
nmap -sA target

# TCP Window scan
nmap -sW target

# TCP Maimon scan
nmap -sM target

# TCP Null scan
nmap -sN target

# TCP FIN scan
nmap -sF target

# TCP Xmas scan
nmap -sX target
```

## Host Discovery

### Discovery Methods
```bash
# ICMP Echo (ping)
nmap -PE target

# ICMP Timestamp
nmap -PP target

# ICMP Address Mask
nmap -PM target

# TCP SYN discovery
nmap -PS22,80,443 target

# TCP ACK discovery
nmap -PA22,80,443 target

# UDP discovery
nmap -PU53,67,68,69,111,123,135,137,138,139,161,162,445,500,514,520,631,996,998 target

# ARP discovery (local network)
nmap -PR target

# Skip host discovery
nmap -Pn target
```

## Service Detection

### Version Detection
```bash
# Basic version detection
nmap -sV target

# Intensity levels (0-9)
nmap -sV --version-intensity 5 target

# Light version detection
nmap -sV --version-light target

# Aggressive version detection
nmap -sV --version-all target

# Version detection with specific ports
nmap -sV -p 22,80,443 target
```

## OS Detection

### Operating System Detection
```bash
# Basic OS detection
nmap -O target

# Aggressive OS detection
nmap -O --osscan-guess target

# Limit OS detection attempts
nmap -O --osscan-limit target

# OS detection with version detection
nmap -O -sV target
```

## Scripting Engine (NSE)

### Basic Script Usage
```bash
# Default scripts
nmap -sC target
nmap --script default target

# Specific script
nmap --script ssh-hostkey target

# Multiple scripts
nmap --script "ssh-*" target

# Script categories
nmap --script auth target
nmap --script broadcast target
nmap --script brute target
nmap --script default target
nmap --script discovery target
nmap --script dos target
nmap --script exploit target
nmap --script external target
nmap --script fuzzer target
nmap --script intrusive target
nmap --script malware target
nmap --script safe target
nmap --script version target
nmap --script vuln target
```

### Popular Scripts
```bash
# Vulnerability detection
nmap --script vuln target

# HTTP enumeration
nmap --script http-enum target
nmap --script http-headers target
nmap --script http-methods target
nmap --script http-title target

# SMB enumeration
nmap --script smb-enum-shares target
nmap --script smb-enum-users target
nmap --script smb-os-discovery target

# DNS enumeration
nmap --script dns-brute target

# SSL/TLS testing
nmap --script ssl-cert target
nmap --script ssl-enum-ciphers target

# Database detection
nmap --script mysql-info target
nmap --script ms-sql-info target

# FTP enumeration
nmap --script ftp-anon target
nmap --script ftp-bounce target
```

### Script Arguments
```bash
# Pass arguments to scripts
nmap --script http-enum --script-args http-enum.basepath='/admin/' target

# Multiple arguments
nmap --script mysql-brute --script-args userdb=users.txt,passdb=passwords.txt target

# Help for specific script
nmap --script-help http-enum
```

## Output Formats

### Standard Output Options
```bash
# Normal output (default)
nmap target

# Verbose output
nmap -v target
nmap -vv target  # Very verbose

# Debug output
nmap -d target
nmap -dd target  # More debug info

# Quiet output
nmap -q target
```

### File Output Formats
```bash
# Normal format
nmap -oN scan.txt target

# XML format
nmap -oX scan.xml target

# Grepable format
nmap -oG scan.grep target

# All formats
nmap -oA scan target  # Creates scan.nmap, scan.xml, scan.gnmap

# Script kiddie format
nmap -oS scan.skid target
```

### Output Control
```bash
# No DNS resolution
nmap -n target

# Force DNS resolution
nmap -R target

# Print host interfaces and routes
nmap --iflist

# Resume scan from output file
nmap --resume scan.xml
```

## Timing and Performance

### Timing Templates
```bash
# Paranoid (0) - Very slow, IDS evasion
nmap -T0 target

# Sneaky (1) - Slow, IDS evasion
nmap -T1 target

# Polite (2) - Slow, less bandwidth
nmap -T2 target

# Normal (3) - Default
nmap -T3 target

# Aggressive (4) - Fast, assume good network
nmap -T4 target

# Insane (5) - Very fast, may miss results
nmap -T5 target
```

### Custom Timing
```bash
# Minimum delay between probes
nmap --scan-delay 1s target

# Maximum delay between probes
nmap --max-scan-delay 10s target

# Minimum packet rate
nmap --min-rate 100 target

# Maximum packet rate
nmap --max-rate 1000 target

# Parallel host scan groups
nmap --min-hostgroup 50 target
nmap --max-hostgroup 100 target

# Parallel port scan
nmap --min-parallelism 10 target
nmap --max-parallelism 100 target
```

## Firewall Evasion

### Fragmentation
```bash
# Fragment packets
nmap -f target

# Custom fragment size
nmap --mtu 16 target  # Must be multiple of 8
```

### Decoy Scans
```bash
# Use decoy IPs
nmap -D 192.168.1.100,192.168.1.101,ME target

# Random decoys
nmap -D RND:10 target
```

### Source Port Manipulation
```bash
# Use specific source port
nmap --source-port 53 target
nmap -g 53 target  # Same as above

# Common source ports: 20, 53, 67, 68, 88, 123, 135, 137, 138, 139, 161, 162, 389, 445, 464, 500
```

### IP Spoofing
```bash
# Spoof source IP
nmap -S 192.168.1.100 target

# Spoof MAC address
nmap --spoof-mac 00:11:22:33:44:55 target
nmap --spoof-mac Dell target  # Use vendor name
nmap --spoof-mac 0 target     # Random MAC
```

### Other Evasion Techniques
```bash
# Append random data
nmap --data-length 25 target

# Use bad checksums
nmap --badsum target

# IPv6 scanning
nmap -6 target

# Proxy through specific interface
nmap -e eth0 target

# Idle/Zombie scan
nmap -sI zombie-host target
```

## Advanced Techniques

### Network Discovery
```bash
# Discover live hosts
nmap -sn 192.168.1.0/24

# ARP scan (local network)
nmap -PR 192.168.1.0/24

# List scan (no packets sent)
nmap -sL 192.168.1.0/24

# Traceroute
nmap --traceroute target
```

### Advanced Port Scanning
```bash
# SCTP INIT scan
nmap -sY target

# SCTP COOKIE ECHO scan
nmap -sZ target

# IP Protocol scan
nmap -sO target

# Bounce scan through FTP
nmap -b ftp-server target
```

### Custom Scripts
```bash
# Update script database
nmap --script-updatedb

# Show script categories
ls /usr/share/nmap/scripts/ | grep -E "\.nse$" | head -20

# Script location
locate "*.nse" | head -10
```

## Common Use Cases

### Network Reconnaissance
```bash
# Quick network overview
nmap -sn 192.168.1.0/24

# Detailed host scan
nmap -A -T4 192.168.1.100

# Service enumeration
nmap -sV -sC 192.168.1.100
```

### Web Server Testing
```bash
# Web server discovery
nmap -p 80,443,8080,8443 --script http-title,http-headers 192.168.1.0/24

# Web vulnerability scan
nmap -p 80,443 --script http-vuln* target

# HTTP methods testing
nmap -p 80,443 --script http-methods target
```

### Database Server Testing
```bash
# MySQL discovery
nmap -p 3306 --script mysql-info,mysql-databases,mysql-users target

# MSSQL discovery
nmap -p 1433 --script ms-sql-info,ms-sql-tables,ms-sql-users target

# PostgreSQL discovery
nmap -p 5432 target
```

### SMB/Windows Testing
```bash
# SMB enumeration
nmap -p 445 --script smb-enum-shares,smb-enum-users,smb-os-discovery target

# Windows vulnerability scan
nmap -p 445 --script smb-vuln* target

# NetBIOS scan
nmap -p 137,138,139 target
```

### Mail Server Testing
```bash
# SMTP enumeration
nmap -p 25,465,587 --script smtp-commands,smtp-enum-users target

# POP3/IMAP testing
nmap -p 110,143,993,995 target
```

## Best Practices

### Scanning Strategy
1. **Start with host discovery**: Use `nmap -sn` to find live hosts
2. **Quick port scan**: Use `nmap -F` for initial assessment
3. **Detailed scan**: Use `nmap -A -T4` for comprehensive analysis
4. **Targeted scanning**: Focus on specific services found

### Performance Optimization
```bash
# For large networks
nmap -T4 --min-hostgroup 50 --max-hostgroup 100 target

# For slow networks
nmap -T2 --scan-delay 1s target

# For reliable results
nmap --max-retries 3 target
```

### Documentation
```bash
# Always save results
nmap -oA company_scan target

# Add comments to XML
nmap -oX scan.xml --stylesheet=nmap.xsl target
```

### Script Safety
- **Safe scripts**: `default`, `safe`, `auth`, `discovery`
- **Potentially intrusive**: `brute`, `exploit`, `dos`, `malware`
- **Test first**: Always test scripts in lab environment

## Legal and Ethical Considerations

### Legal Guidelines
- **Only scan networks you own or have explicit permission to test**
- **Follow responsible disclosure for vulnerabilities found**
- **Respect rate limits and avoid DoS conditions**
- **Document authorization before scanning**

### Ethical Usage
- Use nmap for legitimate security testing only
- Respect privacy and confidentiality
- Follow your organization's security policies
- Consider the impact on network performance

### Common Legal Scenarios
✅ **Legal**: Scanning your own network
✅ **Legal**: Authorized penetration testing
✅ **Legal**: Using scanme.nmap.org (official test target)
❌ **Illegal**: Scanning networks without permission
❌ **Illegal**: Using results for malicious purposes

## Quick Reference Commands

### Essential One-Liners
```bash
# Basic network discovery
nmap -sn 192.168.1.0/24

# Quick port scan
nmap -F target

# Comprehensive scan
nmap -A -T4 target

# Vulnerability scan
nmap --script vuln target

# Service version detection
nmap -sV target

# Stealth SYN scan
nmap -sS target

# UDP scan
nmap -sU --top-ports 1000 target

# OS detection
nmap -O target

# All TCP ports
nmap -p- target

# Web server enumeration
nmap -p 80,443 --script http-enum target
```

### Useful Aliases
Add to your `.bashrc` or `.zshrc`:
```bash
alias nmap-quick='nmap -T4 -F'
alias nmap-intense='nmap -T4 -A -v'
alias nmap-vuln='nmap --script vuln'
alias nmap-web='nmap -p 80,443 --script http-enum,http-title'
alias nmap-ping='nmap -sn'
```

## Additional Resources

- **Official Documentation**: https://nmap.org/book/
- **NSE Script Database**: https://nmap.org/nsedoc/
- **Nmap Reference Guide**: https://nmap.org/book/man.html
- **Community Scripts**: https://github.com/nmap/nmap
- **Practice Target**: scanme.nmap.org

Remember: With great power comes great responsibility. Use nmap ethically and legally!
