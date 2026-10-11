# Monitoring Evidence Index

Existing segmentation screenshots provide the monitoring evidence. They are linked here rather than copied.

| Evidence | Control Tested | Expected Result | Actual Result |
| --- | --- | --- | --- |
| [SEG-11](../Segmentation/SEG-11-Firewall-Blocked-Traffic-Logs.png) | Denial logging | Record denied TCP traffic with source, target and port | UFW BLOCK: source 10.10.10.1, target 10.10.10.101, port 8080, interface enp0s8 |
| [SEG-12](../Segmentation/SEG-12-Security-Incident-Investigation.png) | Security event investigation | Match blocked events with enforced policy and service state | Matching TCP SYN events, active UFW, medium logging, incoming deny and active application service |
| [AUTH-01](../Authentication/AUTH-01-Unauthenticated-Access-Denied.png) | Application request logging and startup chronology | Show application response and service state | Journal contains local GET request returning 401, startup entries and a boot boundary; no centralized alert or brute-force detection test |

This demonstrates local kernel-journal investigation. Centralized ingestion, alert rules, SIEM correlation, retention and time synchronization are not demonstrated. The separate SEC-001 SOC repository is not merged into SEC-008.
