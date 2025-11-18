# 1. Finding Hidden urls in a website
To find hidden URLs, we will use a tool called **dirb**. This tool uses a brute-force approach, by taking a list of potential page names and testing one by one if they exist in your website. This approach works because people use predictable names a lot of times.
You open your terminal and run the command **dirb folowed by the url of the website**
```
dirb http://fakebank.thm
```

# 2. Subdomain Enumeration
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
### Using OSINT
using gooogle search engine. For example if I want to get the subdomains of www.tryhackme.com I'll just search
```
-site:www.tryhackme.com site:*.tryhackme.com
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
```
ffuf -u http://TARGET_IP/ -w /usr/share/wordlists/seclists/Discovery/DNS/namelist.txt -H "Host: FUZZ.example.com" -fs 0
```
# Passive Recon
### whois commad
Used to query WHOIS servers for example registrar of a website, contact information,name server etc
```
whois tryhackme.com
```
### nslookup and dig
Purpose: To find the IP address and other DNS records of a domain name. You can use commands
```
nslooup domain   or   nslookup -type=A domain
```
you can use type=A for ipv4 addresses, type=AAAA for ipv6 addresses, type=MX for mail exchanger, type=CNAME for canonical names etc

for more advanced dns records you can use **dig** ie
```
dig DOMAIN_NAME TYPE (dig example.com MX)
dig example.com @8.8.8.8
```
### DNSDumpster
reconnaissance tool that discovers hidden subdomains and maps out a target's DNS infrastructure quickly and efficiently.
You can find it here https://dnsdumpster.com/

### Shodan.io
It allows you to gather detailed information about a target's network without actively connecting to it yourself.

# 3. Authentication bypass
### username enumeration
This can be done in a signup page to check the usernames that already exist
you run the command 
```
ffuf -w /usr/share/seclists/Usernames/Names/names.txt  \
     -X POST \
     -d "username=FUZZ&email=x&password=x&cpassword=x" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -u URL \
     -mr "username already exists"
```
### Bruteforcing 
After finding valid usernames you can create a file and store those usernames ie valid_usernames.txt and run this command to bruteforce their passwords
```
fuff -w valid_usernames.txt:W1,/usr/share/wordlists/seclists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 \
-X POST \
-d "username=W1&password=W2" \
-H "Content-Type: application/x-www-form-urlencoded" \
-u URL
-fc 200
```
by doing this you ca find a match











