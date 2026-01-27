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









