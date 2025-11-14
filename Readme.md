# Finding Hidden urls in a website
To find hidden URLs, we will use a tool called **dirb**. This tool uses a brute-force approach, by taking a list of potential page names and testing one by one if they exist in your website. This approach works because people use predictable names a lot of times.
You open your terminal and run the command **dirb folowed by the url of the website**
```
dirb http://fakebank.thm
```

# Subdomain Enumeration
It is the process of discovering valid subdomains of a domain.
**Passive Subdomain enumeration** - Does NOT interact directly with the target.It gathers data from public sources.
**Avtive Subdomain enumeration** - Directly interacts with the target

### Using Dnsrecon for Subdomain Enumeration
```
dnsrecon -d example.com -D wordlist.txt -t brt
```
or
```
dnsrecon -t brt -d example.com
```
### Using Sublist3r for Sudomain Enumeration
Sublist3r is a Python tool used to enumerate subdomains using OSINT. 
```
sublist3r -d acmeitsupport.thm
```
### Subdomain enumeration using Virtual host
A virtual host is when a single server (same IP address) hosts multiple websites or subdomains.
we use this tool ffuf
```
ffuf -w wordlist.txt -u http://TARGET_IP/ -H "Host: FUZZ.example.com" -fs 0
```
Explanation:

-H modifies the Host header

FUZZ is replaced by each word in the list

-fs 0 filters size 0 responses
