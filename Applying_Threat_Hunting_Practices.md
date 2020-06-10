- [Threat Hunt Checklist](#threat-hunt_checklist)
  - [Prepare and Execute Threat Hunting](#prepare-and-execute-threat-hunting)
  - [Observe a potential adversary](#observe-a-potential-adversary)
  - [Leverage strong egress detection](#leverage-strong-egress-detection)
  - [Monitor Privileged Accounts](#monitor-privileged-accounts)
  - [Account Life Cycle](#account-life-cycle)
  - [Newly Registered Domains](#newly-registered-domains)
- [Hunting Historical Data Based on Current Intel and Alarms](#hunting-historical-data-based-on-current-intel-and-alarms)
- [Execssive or Multiple Source IPs for User Logins](#execssive-or-multiple-source-ips-for-user-logins)
- [Command and Control Detection](#command-and-control-detection)
  - [Network Based C&C Detection](#network-based-c&c-detection)
  - [Application Content Based C&C Detection](#application-content-based-c&c-detection)

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

## Network Based C&C Detection
Criteria: Well known IP to Site Relationships
````
Explanation: Filtering out IPs from a list of the top one million site list
````
