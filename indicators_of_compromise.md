## Indicators of Compromise and Attack Data Dependencies

An IoC is a piece of forensice data observed on the network, in a log file, a persistence facility, or the operating system that are likely to indicate malicious
activity which can aid security operations or incident responders to detect breaches, malicious activity, misuse, or some other form of attack.

Indicators of Attacl (IoA) differs from an IoC because IoAs focus on what an attacker is attempting to accomplish, not just the malware or observed behavior. IoC's depend on some form of signature or pattern match, like a firewall record with a source IP matching a threat feed. IoA's can be a directed spear phish email, a password spray condition, outbound traffic for tftp (69/UDP), an internal scan observed on the network, rogue access points appearing in the building, or a new hire accessing shares and files en masse that are not part of their job responsibility.

| Indicator      | Data Dependency | 
| :---        |    :----:   |
| Unusual outbound network traffic       | Firewall Logging, including a "default deny" policy, Web Proxy Logging, Protocol mismatch detection from Bro logs, Unusual IP protocols which can be detected by a simple NIDS rule       |
| Account Managment Anomalies   | Central directory, designated account managers, knowledge of privileged accounts, service accounts, consistent account naming and account correlation across various systems. Cross system correlation may require a consistent attribute be added to all systems such as an employee unique identifier        |
| Privilege Account Misue/Anomlaies.    | Inventory of privileged accounts for the directory and systems which reside in "account islands". Account islands are often application specific, and contain very valuable data.               |
| Geographical Account Usage Patterns or Improbabilities                | VPN/Citrix logging, rules to detect "inside in use" vs. "outside detected", IP to geolocation attribution such as MaxMind's data. Source data enrichment may be required so geolocatoin can be added as data arrives|
| Account Usage Attempts for Uknown Accounts| Robust logging across the enterprise, Auditing that records failure conditions along with a reason code, Login activity from non-AD integrated systems               |
| Confirmed Threat Intel "hit"| Intelligence feed(s) and the ability to match an attribute against data arriving in the system, such as an IP address, DNS name, email address, malware hash, etc. |
| First Seen Binary| File system integration or a HIDS tool and an inventory of known/observed binaries, hash list of known binaries. This IoC requires at least a month of observation before it is likely to work properly.|
| Database Query Volume and Velocity Changes| More sophisticated query statistics, DB activity logging, standard deviation detection, and encryption keys to allow for protocol inspection|
| HTML or Website Query Size and Ratio Mismatches (producer to consumer)| Web server and proxy server logging which has enough granularity; depends on ability to get packet size|
| URL Hits Above Baseline| Web server and proxy server logging which has enough granularity to provide the full URL (UDP baed syslog may cut this off)|
| Protocol Abuse; Mismatched Protocol to well-known Ports| IDS or NGFW capable of analyzing network traffic at the application level and analyzing port to application usage|
| Registery (or/etc)/Filesystem Changes Outside of the Change Window| File integrity monitoring such as OSSEC, Tripwire |
| DNS mismatches, fail spikes, DGA queries, excessive TXT queries, rapid name to IP changes (Fastflux)|  High quality DNS audit log with the true source, request/response, and an analysis engine (can be performed on or near the DNS server with a tool like Bro IDS or PassiveDNS)|
| OS Changes of significance outside of the change window| Patch detection, "system" activity logging such as new services, share creation, application install/modification/removal, and other OS state change data|
| Table/Mobile/Phablet or other IoT Device Profile Changes| A MDM solution that can be applied to the IoT device|
| Unexplained Disk Usage/Volume Changes |  Operating system integration with a performance monitor tool |
| Web Browsing that does not Match Human Page Consumtion speed, click rate, and habits | Web proxy logging |
| Suspicious User Agents | User agent logging in a proxy, NIDS              |
