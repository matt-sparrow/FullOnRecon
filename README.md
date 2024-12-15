# FullOnRecon
FullOnRecon is a bash script i've written to automate the initial portions of an external penetration test involving domains.  I did not initially write this to publish.  Please take this and modify it to fit your needs.  I will not be maintaining or modifying this from its current version, and already went through and heavily commented it so outsiders could follow my likely impractical logic.  There are a lot of output files, but they're all evidence.  Some things to consider:

* masscan is run with --rate 1250.  You may want to change this based on preferences
* masscan is run with -p-.  You may want to use --top-ports 1000 or something else.
* nmap is run with -T4 which you may or may not want to change.
* nuclei is setup to run with a pretty default scan.  You might want to tweak that line in multiple ways
* This does display certain output that I've determined I want to see, modify to suit your preferences!

I follow all of this up with tools like httpx, eyewitness, katana, etc. as appropriate but they're situation dependent.

# Requirements
* masscan (https://github.com/robertdavidgraham/masscan)
* nmap (https://github.com/nmap/nmap)
* subfinder (https://github.com/projectdiscovery/subfinder)
* nuclei (https://github.com/projectdiscovery/nuclei)

# Usage
chmod +x fullonrecon.sh

sudo fullonrecon {domain} OR {input-domains.txt}

# Output
This will output multiple files that are useful in documenting for reporting, and they are as follows:
* {domain}-subs.txt - List of subdomains that were discovered through passive scans using subfinder.  Absolutely no validation or additional checks done.
* {domain}-subs-resolution.txt - List of subdomains with their IP resolutions in the format of subdomain:ip.  If there is no IP resolution, it will just be blank after the colon.
* {domain}-subs-with-ip.txt - List of subdomains that actually resolved to an IP
* {domain}-targets.txt - List of IPs that were resolved from subdomains
* {domain}-masscan.out - Results from masscan (tee'd output)
* {domain}-responding_masscan_hosts.txt - IPs that had an open port.  Masscan is run with -p- so it scans all ports.
* {domain}-service_scans.* - The output from nmap with the -sV -Pn -oA flags
* {domain}-nuclei-output.json - Json output from nuclei scans on the IPs
* {domain}-dangling-dns.txt - DNS entries that were assigned to an IP, but the IP did not return anything in the masscan results.
* results.tar.gz - Everything gets wrapped up into a pretty little package for you to preserve.

# Sample of output:

[*] Starting processing for domain: mattsparrow.com

[*] Running subfinder for domain: mattsparrow.com

cpcontacts.mattsparrow.com

webdisk.mattsparrow.com

cpcalendars.mattsparrow.com

mattsparrow.com

www.mattsparrow.com

mail.mattsparrow.com

webmail.mattsparrow.com

cpanel.mattsparrow.com

[==                  ]  10% Complete (Processing: mattsparrow.com)[*] Resolving subdomains to IP addresses

[====                ]  20% Complete (Processing: mattsparrow.com)[*] Filtering subdomains with valid IP addresses

[======              ]  30% Complete (Processing: mattsparrow.com)[*] Extracting IP addresses to targets.txt

[========            ]  40% Complete (Processing: mattsparrow.com)[*] Running masscan on targets

Starting masscan 1.3.2 (http://bit.ly/14GZzcT) at 2024-12-15 16:25:04 GMT

Initiating SYN Stealth Scan

Scanning 1 hosts [65535 ports/host]

Discovered open port 2079/tcp on 198.20.90.34

Discovered open port 465/tcp on 198.20.90.34

Discovered open port 26/tcp on 198.20.90.34

Discovered open port 2082/tcp on 198.20.90.34

Discovered open port 993/tcp on 198.20.90.34

Discovered open port 110/tcp on 198.20.90.34

Discovered open port 2087/tcp on 198.20.90.34

Discovered open port 587/tcp on 198.20.90.34

Discovered open port 21/tcp on 198.20.90.34

Discovered open port 3306/tcp on 198.20.90.34

Discovered open port 443/tcp on 198.20.90.34

Discovered open port 1030/tcp on 198.20.90.34

Discovered open port 143/tcp on 198.20.90.34

Discovered open port 2096/tcp on 198.20.90.34

Discovered open port 2086/tcp on 198.20.90.34

Discovered open port 53/tcp on 198.20.90.34

Discovered open port 2083/tcp on 198.20.90.34

Discovered open port 2078/tcp on 198.20.90.34

Discovered open port 2095/tcp on 198.20.90.34

Discovered open port 80/tcp on 198.20.90.34

Discovered open port 995/tcp on 198.20.90.34

Discovered open port 2077/tcp on 198.20.90.34

Discovered open port 2080/tcp on 198.20.90.34

[==========          ]  50% Complete (Processing: mattsparrow.com)[*] Extracting responding hosts from masscan results

[============        ]  60% Complete (Processing: mattsparrow.com)[*] Extracting unique ports from masscan results

[==============      ]  70% Complete (Processing: mattsparrow.com)[*] Running nmap service scan on responding hosts

[================    ]  80% Complete (Processing: mattsparrow.com)[*] Running nuclei scans

[==================  ]  90% Complete (Processing: mattsparrow.com)[*] Identifying dangling DNS entries

[====================] 100% Complete (Processing: mattsparrow.com)[*] Completed processing for domain: mattsparrow.com


[*] Creating an archive of all results (results.tar.gz)

[*] All tasks completed. Results archived in results.tar.gz

