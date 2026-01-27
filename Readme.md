## Penetration testing methodology
Phase 1: Reconnaissance 
Phase 2: Vulnerability Identification
Phase 3: Exploitation and Compromise
Phase 4: Post-Exploitation
Phase 5: Documentation and Reporting

### Phase 1: Reconnaissance
#### Asset Discovery
**Domain Idntification and Analysis**

We ca Discover Domains through:
(i) Google Search
(ii) Google Dorking for example

```
site:linkedin.com "Company Name" domain or site:crunchbase.com "Company Name"
```
```
site:*.companyname.com or "Company Name" site:*.com OR site:*.net OR site:*.org
```
(iii) Using **whois** command ie
```
whois "company name""
```

(iv) We can also use sites like **https://crt.sh/** to check for subdomains

#### DNS Enumeration
DNS enumeration is the process of querying DNS records to gather information about a target’s infrastructure, services, and attack surface/n.
The dig command provides comprehensive DNS enumeration capabilities essential for infrastructure discovery:

Basic DNS Record Enumeration:
```
# Comprehensive DNS record discovery
dig example.com ANY                    # All available records
dig example.com A                      # IPv4 addresses
dig example.com AAAA                   # IPv6 addresses
dig example.com MX                     # Mail servers
dig example.com TXT                    # Text records
dig example.com NS                     # Name servers
dig example.com SOA                    # Start of Authority
dig -x {IP Address}                        # Linked domain

# Advanced DNS analysis
dig +short example.com MX              # Concise output
dig +trace example.com                 # Trace DNS resolution path
dig @8.8.8.8 example.com              # Use specific DNS server
dig +noall +answer example.com ANY     # Clean output format
```

**DNS Zone Transfer Attempts:**

Zone transfers can reveal complete DNS zone information, including internal hostnames and infrastructure details:

```
# Identify name servers
dig example.com NS

# Attempt zone transfer from each name server
dig @ns1.example.com example.com AXFR
dig @ns2.example.com example.com AXFR

# Alternative zone transfer syntax
dig AXFR example.com @ns1.example.com
```

**DNS Enumeration with nslookup**
The nslookup command provides alternative DNS enumeration capabilities with different output formats:
```
# Interactive mode
nslookup
> set type=MX
> example.com
> set type=TXT
> example.com
> exit

# Command-line mode
nslookup -type=A example.com
nslookup -type=MX example.com
nslookup -type=TXT example.com
nslookup -type=NS example.com

# Reverse DNS lookups
nslookup 192.168.1.1
nslookup -type=PTR 1.1.168.192.in-addr.arpa
```
**Advanced nslookup Techniques:**
```
# Using specific DNS servers
nslookup example.com 8.8.8.8
nslookup -type=MX example.com 1.1.1.1

# Zone transfer attempts
nslookup -type=AXFR example.com ns1.example.com

# Detailed DNS server information
nslookup -debug example.com
```
#### Subdomain Enumeration: Expanding the Attack Surface
**Passive Subdomain discovery :** You do not touch the target’s servers directly. You only use public data

**(a) Certificate Transparency Analysis**
Just go to this link **https://crt.sh** to discover subdomains/n
**(b) Using curl (automation)**
```
curl -s "https://crt.sh/?q=example.com&output=json” | jq -r ‘.[].namevalue’ | sort -u curl -s “https://crt.sh/?q=%.example.com&output=json” | jq -r ‘.[].namevalue’ | sort -u

```
This:

Downloads certificate data

Extracts subdomain names

Removes duplicates

**(c) Historical certificate analysis**
```
curl -s “https://crt.sh/?q=example.com&output=json” | jq -r ‘.[] | “(.notbefore) (.namevalue)”’ | sort “`
```
This tells you:

When a subdomain first appeared

Old / deprecated systems

**(d) Subfinder: Comprehensive Passive Discovery**

Subfinder aggregates subdomain information from multiple passive sources:

```
# Basic subfinder usage
subfinder -d example.com

# Multiple sources and detailed output
subfinder -d example.com -all -v

# Output to file for analysis
subfinder -d example.com -o subdomains.txt

# Multiple domains using a domain input list
subfinder -dL domains.txt -o all_subdomains.txt

# Use specific sources (might need API keys)
subfinder -d example.com -sources censys,virustotal,shodan
```
**(e) Amass: Advanced Asset Discovery**
Amass provides comprehensive asset discovery combining passive and active techniques:

```
# Passive enumeration
amass enum -passive -d example.com

# Active enumeration (more thorough)
amass enum -active -d example.com

# Brute force discovery
amass enum -brute -d example.com

# Output with detailed information
amass enum -d example.com -o amass_results.txt -v

# Multiple domains with configuration
amass enum -df domains.txt -config config.ini
```
#### Active Subdomain Discovery
Active subdomain enumeration directly queries DNS servers and may use brute force techniques to discover subdomains.

**Assetfinder: Lightweight Discovery**
Assetfinder provides fast subdomain discovery with minimal dependencies:

```
# Basic assetfinder usage
assetfinder example.com

# Include subdomains of subdomains
assetfinder --subs-only example.com

# Combine with other tools
assetfinder example.com | sort -u > assetfinder_results.txt
```

#### Infrastructure Footprinting: Connecting the Dots
Infrastructure footprinting involves analyzing discovered assets to understand their relationships, hosting patterns, and network architecture.
**IP Address Analysis and Network Mapping**
**IP to Domain Resolution:**
```
# Reverse DNS lookups
dig -x 192.168.1.1
nslookup 192.168.1.1

# Batch reverse DNS using a script
for ip in $(seq 1 254); do
    dig -x 192.168.1.$ip +short | grep -v "^$"
done
```
**Network Range Discovery:**
```
# WHOIS network information
whois 192.168.1.1 | grep -E "(NetRange|CIDR|inetnum)"

# Network range enumeration
whois -h whois.arin.net 192.168.1.1
whois -h whois.ripe.net 192.168.1.1
whois -h whois.apnic.net 192.168.1.1
```
**IPv4 Lookup Tools:**
After you get an IP address, you should look it up to understand who owns it and where it is.

An IP by itself (like 192.168.1.1) doesn’t tell you much.
IPv4 lookup tools add context.
```
# Using online IPv4 lookup services
curl -s "https://ipv4info.com/ip-address/192.168.1.1" | grep -E "(Organization|ISP|Country)"
```
Step by step:

1️⃣ curl -s

Fetches a web page silently (no progress output)

2️⃣ https://ipv4info.com/ip-address/192.168.1.1

Queries an online IP information service

3️⃣ grep -E "(Organization|ISP|Country)"

Filters the page to show only:

Organization

ISP

Country







