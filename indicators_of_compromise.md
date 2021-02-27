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
| Confirmed Threat Intel "hit"                  |               |
| First Seen Binary|               |
| Database Query Volume and Velocity Changes|               |
| HTML or Website Query Size and Ratio Mismatches (producer to consumer)                 |               |
| URL Hits Above Baseline|              |
| Protocol Abuse; Mismatched Protocol to well-known Ports|               |
| Registery (or/etc)/Filesystem Changes Outside of the Change Window                |               |
| DNS mismatches, fail spikes, DGA queries, excessive TXT queries, rapid name to IP changes (Fastflux)                |               |
| OS Changes of significance outside of the change window|               |
| Table/Mobile/Phablet or other IoT Device Profile Changes|               |
| Unexplained Disk Usage/Volume Changes                 |               |
| Web Browsing that does not Match Human Page Consumtion speed, click rate, and habits                 |               |
| Suspicious User Agents                 |               |
