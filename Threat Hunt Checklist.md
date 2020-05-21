
# Threat Hunt Checklist

## 1) Prepare and Execute Threat Hunting
* Search for signs of Command and Control
  * Look for beacons using a tool like Real Intelligence Threat Analytics
  * List the top 20 IPs with the greatest number of connections, the longest connection time and most about of data moved. Ensure that any system in all three lists has a well understood communication pattern.
  * Look for long running transactions(**> 8hrs**)
  * DNS responses with high entropy domain names.
  * Uknown user agents observed.
  * SSL interactions with known-malicious/self-signed sites.
  * Dynamic DNS queries to D-DNS providers.
  * Long DNS queries, DNS txt queries, execssive DNS failed queries
  
## 2) Observe a potential adversary as they would go after your "crown jewels"
* Review the event types and alerts generated from systems that contain the most sensitive data.
* An adversary will compromise the environment through the desktop and moves laterally, so search out 4624 authentication events from within systems on the network and look for odd patterns.
* "Live off the land", leverage PowerShell and built in commands. Review the output of sysmon and 4688 data for the invoking process, the invoked process, and the command line used for PowerShell and cmd.exe processes.

## 3) Leverage strong egress detection
* Document which systems should be used for specific services and look for systems that violate those rules such as DNS, FTP, email (SMTP, IMAP)
* Monitor all DMZ assets for initial outbound attempts - they should normally respond to inbound, if there are outbound it should be very well understood.

## 4) Monitor Privileged Accounts
* Get the current membership of elevated groups and then review actions taken by these users.
* Activities like scheduling tasks should clearly relate to system management.

## 5) Account Life Cycle
* Ensure that account life cycle events to elevated groups are fully monitored and are supported by job roles.

## 6) Newly Registered Domains
* Pull the list of domains at 1am and run the prior days queried daomins against this list from your proxy or URL filter to determine if a user successfully connected to one of these.
