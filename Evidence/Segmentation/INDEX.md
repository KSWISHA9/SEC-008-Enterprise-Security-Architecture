# Segmentation Evidence Index

This index documents the SEC-008 network segmentation and firewall evidence. Each screenshot was reviewed before being copied into this folder. The actual result only describes what the screenshot demonstrates.

These are earlier baseline/segmentation captures. Later authentication behavior is in [Authentication/INDEX.md](../Authentication/INDEX.md). AUTH-01's journal boot boundary corroborates earlier reboot chronology, but does not add remote persistence tests to SEG-10 itself.

| ID | Screenshot | Control Tested | Expected Result | Actual Result |
| --- | --- | --- | --- | --- |
| SEG-01 | [Baseline Kali to Ubuntu](SEG-01-Baseline-Kali-to-Ubuntu.png) | Baseline reachability | Target responds; identify available services | `10.10.10.101` replies to 3/3 pings; TCP 22 is open, TCP 80 and 443 are closed. Port 8080 is not tested here |
| SEG-02 | [Unrestricted Application Access](SEG-02-Unrestricted-Application-Access.png) | Pre-segmentation application exposure | Application service should be reachable before least-privilege firewall enforcement | Screenshot shows the internal training application responding over HTTP before restrictive segmentation |
| SEG-03 | [Firewall Enforcement Filtered Ports](SEG-03-Firewall-Enforcement.png) | Initial restrictive firewall test | Tested ports become unreachable | Kali scan reports TCP 22 and 8080 filtered; curl times out. This precedes the later approved application-access test |
| SEG-04 | [Authorized SSH Access](SEG-04-Authorized-SSH-Access.png) | Windows administrative SSH | Administrative connection reaches an authenticated shell | Windows runs `ssh omniverse-ubuntu`; Ubuntu banner and `analyst@ubuntu` prompt are visible. The screenshot does not expose the SSH alias configuration |
| SEG-05 | [Least-Privilege Application Access](SEG-05-Least-Privilege-Application-Access.png) | Approved application access | Kali reaches TCP 8080 while SSH remains filtered | TCP 22 is filtered; TCP 8080 is open; curl returns HTTP 200 and application HTML. Client IP is not shown here; SEG-06 identifies the configured allowed source |
| SEG-06 | [UFW Policy and Application Listener](SEG-06-UFW-Policy-and-Application-Listener.png) | Rules and listener | Active UFW; interface-scoped SSH; source-scoped app access | UFW active; SSH allowed from Anywhere on enp0s3 (IPv4/IPv6); 10.10.10.100 allowed to 10.10.10.101:8080 on enp0s8; Python listener present. Default policy is shown in SEG-12, not this numbered output |
| SEG-07 | [Unauthorized Client Blocked](SEG-07-Unauthorized-Client-Blocked.png) | Windows application denial | TCP 8080 connection fails from the non-approved source | SourceAddress 10.10.10.1; target 10.10.10.101:8080; ping succeeds; TcpTestSucceeded is False. SEG-11 independently shows matching firewall blocks |
| SEG-08 | [Network Configuration and Local HTTP Check](SEG-08-Persistent-Network-Configuration.png) | Host-local operational checks | Expected addresses, active rules, local HTTP response | enp0s3 is 10.0.2.15/24; enp0s8 is 10.10.10.101/24; UFW active; host-local curl returns HTTP 200. No persistent configuration file, remote-client test, or reboot marker is visible |
| SEG-09 | [Systemd Managed Application](SEG-09-Systemd-Managed-Application.png) | Service conversion and enablement | Managed service active and enabled; HTTP responds | Manual PID stopped; enablement symlink created; service active/enabled; HTTP 200. Immediate ss query has no listener row; do not claim listener verification from this frame |
| SEG-10 | [Reported Reboot Validation](SEG-10-Reboot-Persistence-Validation.png) | Reported reboot persistence | After a demonstrated reboot, policy and service remain functional | Capture labeled reboot validation shows expected IPs, UFW active, service active/enabled, Python listener, and local HTTP 200. Reboot itself and remote post-reboot allow/deny tests are not independently demonstrated |
| SEG-11 | [Firewall Denial Logging](SEG-11-Firewall-Blocked-Traffic-Logs.png) | Denial audit trail | Blocked connection logged with source, target and port | UFW BLOCK on enp0s8; SRC=10.10.10.1; DST=10.10.10.101; TCP SYN DPT=8080 |
| SEG-12 | [Final Policy and Security Event Validation](SEG-12-Security-Incident-Investigation.png) | Event correlation, defaults and service | Match denied traffic to active policy and service | Matching UFW blocks; logging on (medium); default deny incoming, allow outgoing, routed disabled; scoped allows; service active. No listener output or successful remote HTTP test appears in this frame |

## Evidence Handling Notes

- Original source screenshots were preserved.
- Screenshots were copied into this folder with standardized names.
- Duplicate source screenshots were identified using SHA-256 hashes before organization.
- No screenshot was renamed as passing a control unless the screenshot supported that result.
- The DNS domain-root issue is tracked separately as an open finding and is not marked resolved in this index.

## Supplemental Captures

- [Approved port scan alternate](Supplemental/SEG-05-Approved-Port-Scan-Alternate.png): TCP 22 filtered and TCP 8080 open; no HTTP response in this capture. The original filename overstated the evidence when considered alone.
- [Additional authorized HTTP response](Supplemental/SEG-05-Authorized-HTTP-Response-Additional.png): Kali curl returns HTTP 200 and application HTML.
- [Source provenance](source-provenance.csv): full SHA-256 matches between each organized file and every matching source screenshot. Matching bytes establish copy integrity, not authenticity of the lab result.
