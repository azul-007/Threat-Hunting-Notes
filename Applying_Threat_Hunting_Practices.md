- [Threat Hunt Checklist](#threat-hunt_checklist)
  - [Prepare and Execute Threat Hunting](#prepare-and-execute-threat-hunting)
  - [Observe a potential adversary](#observe-a-potential-adversary)
  - [Leverage strong egress detection](#leverage-strong-egress-detection)
  - [Monitor Privileged Accounts](#monitor-privileged-accounts)
  - [Account Life Cycle](#account-life-cycle)
  - [Newly Registered Domains](#newly-registered-domains)
- [Hunting Historical Data Based on Current Intel and Alarms](#hunting-historical-data-based-on-current-intel-and-alarms)
- [Execssive or Multiple Source IPs for User Logins](#execssive-or-multiple-source-ips-for-user-logins)

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

**Example**
If a malicious IP is ID'd, compare that IP to recent firewall or Bro connection logs. Get the DNS name, assuming DNS logs, sift through and find systems that queried for that domain name or compare newly indefinite DNS names with recent proxy server logs.

# Execssive or Multiple Source IPs for User Logins
