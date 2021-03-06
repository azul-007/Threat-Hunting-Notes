- [Threat Hunt Checklist](#threat-hunt-checklist)
  - [Prepare and Execute Threat Hunting](#prepare-and-execute-threat-hunting)
  - [Observe a potential adversary](#observe-a-potential-adversary)
  - [Leverage strong egress detection](#leverage-strong-egress-detection)
  - [Monitor Privileged Accounts](#monitor-privileged-accounts)
  - [Account Life Cycle](#account-life-cycle)
  - [Newly Registered Domains](#newly-registered-domains)
- [Hunting Historical Data Based on Current Intel and Alarms](#hunting-historical-data-based-on-current-intel-and-alarms)
- [Execssive or Multiple Source IPs for User Logins](#execssive-or-multiple-source-ips-for-user-logins)
- [Command and Control Detection](#command-and-control-detection)
  - [Network Based C2 Detection](#network-based-c2-detection)
  - [Application Content Based C2 Detection](#application-content-based-c2-detection)
- [Lateral Movement or Lateral Traversal](#lateral-movement-or-lateral-traversal)


# Threat Hunt Checklist

## Prepare and Execute Threat Hunting
* Search for signs of Command and Control
  * Look for beacons using a tool like Real Intelligence Threat Analytics
  * List the top 20 IPs with the greatest number of connections, the longest connection time and most about of data moved. Ensure that any system in all three lists has a well understood communication pattern.
  * Look for long running transactions(**> 8hrs**)
  * DNS responses with high entropy domain names.
  * Uknown user agents observed.
  * SSL interactions with known-malicious/self-signed sites.
  * Dynamic DNS queries to D-DNS providers.
  * Long DNS queries, DNS txt queries, execssive DNS failed queries
  
## Observe a potential adversary
* Review the event types and alerts generated from systems that contain the most sensitive data.
* An adversary will compromise the environment through the desktop and moves laterally, so search out 4624 authentication events from within systems on the network and look for odd patterns.
* "Live off the land", leverage PowerShell and built in commands. Review the output of sysmon and 4688 data for the invoking process, the invoked process, and the command line used for PowerShell and cmd.exe processes.

## Leverage strong egress detection
* Document which systems should be used for specific services and look for systems that violate those rules such as DNS, FTP, email (SMTP, IMAP)
* Monitor all DMZ assets for initial outbound attempts - they should normally respond to inbound, if there are outbound it should be very well understood.

## Monitor Privileged Accounts
* Get the current membership of elevated groups and then review actions taken by these users.
* Activities like scheduling tasks should clearly relate to system management.

## Account Life Cycle
* Ensure that account life cycle events to elevated groups are fully monitored and are supported by job roles.

## Newly Registered Domains
* Pull the list of domains at 1am and run the prior days queried daomins against this list from your proxy or URL filter to determine if a user successfully connected to one of these.

# Hunting Historical Data Based on Current Intel and Alarms
Analyzing prior period data can trigger analysis for yesterday or the prior week in the condition existed. Run a script like
PulledPork to update the rule base and the NIDS will be able to detect new patterns.

Take conditions that are updated in the daily rule update feed, and then reprocess the last 3 to 7 days of PCAP data or another appropriate data source.

**Example:**
If a malicious IP is ID'd, compare that IP to recent firewall or Bro connection logs. Get the DNS name, assuming DNS logs, sift through and find systems that queried for that domain name or compare newly indefinite DNS names with recent proxy server logs.

# Execssive or Multiple Source IPs for User Logins
A users account is often used from just a few source addresses - usually two or three. For a desktop user, the source IP is
their primary workstation, maybe a training computer, secondary PC or a tablet/smartphone.

**Example:**
A few IPs visible externally to the Citrix site. And a few internal addresses, such as their primary workstation, the training room, and maybe a conference room. Windows 2008 and forward records the source IP and/or source system name when someone logs in via RDP, or in the case of a domain controller, logs in locally and is authenticated by the domain. You can run a report, get the data in CSV, load it up in Excel and create a pivot table. Then sort in descending order to see if anyone logs in from more than a few addresses.

# Command and Control Detection
Users browsing habitsdo not generally have regular, definable heartbeat access patterns and have a significantly higher ratio of data received from a web server as opposed to the amount sent by a browser.

C&C communications patterns have a definable heartbeat - they pulse, beacon or communcate following a regular pattern. The botnets often use the following to communicate back to their servers: encrypted networks, traffic embedded within ICMP payloads, DNC payloads, artificially generated DNS node names, P2P file swapping networks, gmail exchange and other instant messaging programs. Domain fronting is a more sophisticated techniqued, where 

## Network Based C2 Detection
**Criteria:** Well known IP to Site Relationships
````
Explanation: Filtering out IPs from a list of the top one million site list
````

**Criteria:** IP Reputation
````
Explanation: New site to IP relationship: A site registered within the last 7 days. 
A newly observed IP for your site. Sit to IP changes. Most sites don't change their IPs frequently.
````

**Criteria:** IP Validated Poor Reputation
````
Several threat analysys services such as OTX monitor IPs for malicious activity and maintain 
lists of IP addresses known to be involved in malicious activity.
````

**Criteria:** Low DNS TTL and DNS to IP changes
````
Historically, a 24hr DNS TTL value was quite common. Today, TTLs may be arbitrarily set low for sites in order to
improve disaster recovery operations, or support DNS round robin. If DNS to IP relationships are set low, and they
change to a new IP or a new autonomous system identified network in an unexplainable way, then the DNS name is likely
involved in botnets using a FastFlux technique.
````

**Criteria:** DNS Queries, new names or very low frequency DNS names and DGA
````
A top one million DNS query list can be used to great effect as a reduction tool. Two examples are the Majestic list 
or the Cisco Umbrella list. 
````


**Criteria:** DNS queries to Dynamic DNS Providers
````
For most businesses, the use of Dynamic DNS will be minimal at best. Review queries answered by DDNS providers.
You will beed a DDNS provider list. 
````

## Application Content Based C2 Detection
**Criteria:** File Tranmissions
````
FTP and HTTP/S upload type file transfers are a normal occurrence for some segment of the population - both user 
workstations and servers. When an internal IP sends data significantly above its threshold, then the 
destination should be checked and possibly the user queried.

As a secondary indicator, end users are more likely to send data via FTP, SFTP, or HTTP/S uploads during working hours, 
while batch processes are more frqequent overnight. Note that SFTP is an extensino to SSH. While it may look 
like remote access there will be a distinct difference in communication volume by direction.
````
**Criteria:** Protocol Violation or Mismatch
````
If outbound HTTP/S traffic is observed on nonstandard web server ports and a technical component can detect this protocol
mismatch condition, it should be investigated to determine if the traffic supports a real organizational requirement.


While these are not perfect or account for every possibility, RFCs do provide a basis for applications and network services
to communicate. Violations to these rules may indicate C&C usage, or others issues. For example, numerous protocols have had
tunneling capabilities built that can use a data field or a normal communication capability for hidden communication. Or
outbound traffic carried over 443/TCP which is not HTTP and which did not begin with a TLS exchange.
````
# Lateral Movement or Lateral Traversal
