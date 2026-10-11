# SECX-001 - Enterprise Security Architecture & Zero Trust

**Status: In progress.** Segmentation, local application authentication, and controlled service recovery have been documented; remaining risks and validation work are still open.

## Project Overview

This project evaluates host-based microsegmentation for an internal application in a VirtualBox training lab. The objective is to reduce unnecessary service exposure, permit the approved application client, retain administrative access, investigate denied connections, and assess service and firewall persistence.

The completed evidence demonstrates host-based microsegmentation, local HTTP Basic application authentication, operational checks, and a controlled application stop/restart. It is not full enterprise Zero Trust: TLS, enterprise identity integration, device posture and continuous access evaluation are not demonstrated. Basic credentials and application content travel over unencrypted HTTP. Reboot evidence is corroborated by a journal boot boundary, but paired boot checks and remote policy retesting remain incomplete.

This repository is separate from the existing SEC-001 SOC project and SEC-002 vulnerability management project. SECX-001 is focused on security architecture, design validation, control evidence, and open findings.

## Lab Environment

| Component | Role | Evidence-supported details |
| --- | --- | --- |
| Ubuntu 24.04.3 LTS | Protected application server | enp0s8: 10.10.10.101/24; enp0s3: 10.0.2.15/24; UFW; systemd |
| Kali Linux | Approved application test client | Nmap and curl tests; firewall allowlist specifies 10.10.10.100; source address needs a separate inventory capture |
| Windows client | Negative application test and administrative access | Denied test source 10.10.10.1; SSH through the omniverse-ubuntu alias |
| OV-DC01 | AD/DNS investigation host | DNS server 10.10.10.10; corp.omniverse.com; separate unresolved DNS finding |
| Internal training application | Protected workload | Earlier Python SimpleHTTP baseline; current Python auth_server.py, HTTP Basic, TCP 8080 on 10.10.10.101; no TLS |

## Architecture

UFW enforces the host network boundary. A second application boundary requires local HTTP Basic credentials from clients that can reach the listener. Network approval alone does not grant application access. The NAT-facing administration interface is separate from the lab-facing application path. These controls do not establish VLAN isolation or enterprise identity-aware Zero Trust.

- [Architecture and Traffic Flows](Architecture/Architecture.md)
- [Security Findings Register](Findings/REGISTER.md)
- [Full Evidence Index](Evidence/INDEX.md)

## Validation Methodology

1. Establish reachability and service exposure with ping, Nmap and curl.
2. Compare the unrestricted baseline with initial firewall enforcement.
3. Test approved application access and administrative SSH separately.
4. Test the unauthorized Windows client and correlate its source, target and port with UFW block logs.
5. Inspect runtime interfaces, firewall policy, service active/enabled state and application response.
6. Review the reported reboot capture, recording what it proves and what still needs confirmation.
7. Compare local and Kali requests without credentials, with invalid credentials and with valid credentials; verify application content separately from HTTP status.
8. Review the controlled stop/start sequence, checking service state, socket, HTTP response and remote negative tests. Do not substitute earlier valid-credential evidence for a post-recovery retest.

Results below come from preserved screenshots, not tests rerun during repository maintenance. SHA-256 comparisons verify unchanged copies; they do not independently establish when or how each test was performed.

## Test Results

| Test | Result | Evidence |
| --- | --- | --- |
| Baseline host reachability | 3/3 ping replies; SSH open, 80/443 closed | SEG-01 |
| Application baseline | 22/8080 open; HTTP 200 | SEG-02 |
| Initial restrictive rules | 22/8080 filtered; curl timeout | SEG-03 |
| Administrative SSH | Authenticated Ubuntu shell visible | SEG-04 |
| Approved application client | 8080 open and HTTP 200; SSH filtered on lab path | SEG-05 |
| Unauthorized Windows client | TCP 8080 fails while ping succeeds | SEG-07 |
| Active default-deny policy and logging | Incoming deny; outgoing allow; routed disabled; medium logging | SEG-12 |
| Runtime network state | Expected interfaces and addresses visible; stored configuration not shown | SEG-08 |
| Managed application | Service active/enabled and HTTP 200 | SEG-09 |
| Reported reboot persistence | SEG-10 operational state; AUTH-01 journal shows a boot boundary and subsequent service start; paired checks and remote persistence retests incomplete | SEG-10, AUTH-01 |
| Security event investigation | Denied TCP SYN events matched to source 10.10.10.1 and port 8080 | SEG-11/12 |
| Recovery preparation | VirtualBox snapshot creation/listing succeeds; backup restore not tested | Legacy snapshot image |
| Ubuntu authentication | No credentials: 401; valid Python request: 200, content verified | AUTH-01/02 |
| Kali authentication | Invalid credentials: 401; valid Python request: 200, content verified | AUTH-03/04 |
| Kali unauthenticated request | Pre-recovery result operator-reported; post-recovery 401 screenshot verified | REC-03 |
| Windows network denial during authentication phase | TCP 8080 blocked from 10.10.10.1 | AUTH-05 |
| Controlled application outage | Inactive; no listener; HTTP 000 / curl exit 7 | REC-01 outage |
| Application restoration | Active; listener restored; unauthenticated 401 | REC-02 |
| Remote post-recovery checks | Kali no/invalid credentials: 401; ping 2/2; valid-credential retest missing | REC-03 |

## Completed Work

### Host-Based Microsegmentation

Implemented a host-based segmentation control on the Ubuntu application host using UFW. The lab moved from broad application exposure to least-privilege access based on authorized source IP and required service ports.

Evidence:

- [SEG-01 - Baseline Kali to Ubuntu](Evidence/Segmentation/SEG-01-Baseline-Kali-to-Ubuntu.png)
- [SEG-02 - Unrestricted Application Access](Evidence/Segmentation/SEG-02-Unrestricted-Application-Access.png)
- [SEG-03 - Firewall Enforcement Filtered Ports](Evidence/Segmentation/SEG-03-Firewall-Enforcement.png)

### Default-Deny UFW Rules

The final verbose UFW output shows an active firewall, default deny for incoming traffic, allow for outgoing traffic, and routed traffic disabled. TCP 8080 is allowed on `enp0s8` only from `10.10.10.100` to `10.10.10.101`. SSH is allowed on the NAT-facing `enp0s3` from any source, including IPv6; it is interface-scoped rather than source-IP-scoped.

Evidence:

- [SEG-06 - UFW Policy and Application Listener](Evidence/Segmentation/SEG-06-UFW-Policy-and-Application-Listener.png)
- [SEG-08 - Persistent Network Configuration](Evidence/Segmentation/SEG-08-Persistent-Network-Configuration.png)
- [SEG-12 - Final Policy and Security Event Validation](Evidence/Segmentation/SEG-12-Security-Incident-Investigation.png)

### Authorized Application Access

Validated that the approved Kali client could access the permitted application service on the Ubuntu host after segmentation rules were applied. The evidence shows the application listener responding to the authorized client.

Evidence:

- [SEG-05 - Least-Privilege Application Access](Evidence/Segmentation/SEG-05-Least-Privilege-Application-Access.png)

### Unauthorized-Client Denial

Validated that an unauthorized Windows client could not connect to the protected application port. The evidence shows the TCP test failed from the unauthorized source.

Evidence:

- [SEG-07 - Unauthorized Client Blocked](Evidence/Segmentation/SEG-07-Unauthorized-Client-Blocked.png)

### Firewall Logging

Collected UFW block logs showing TCP SYN attempts from the Windows source `10.10.10.1` to `10.10.10.101:8080` on `enp0s8`. Together with SEG-07, these support the observed unauthorized-client denial. Logging is enabled at medium level in SEG-12.

Evidence:

- [SEG-11 - Firewall Denial Logging](Evidence/Segmentation/SEG-11-Firewall-Blocked-Traffic-Logs.png)

### Systemd Service Management

Configured the internal training application as `secx001-app.service`. SEG-09 shows the manual process being stopped, `systemctl enable --now`, an enablement symlink, `active` and `enabled` output, and HTTP 200. Its immediate listener query has no result row; SEG-10 separately shows a Python listener bound to `10.10.10.101:8080`.

Evidence:

- [SEG-09 - Systemd Managed Application](Evidence/Segmentation/SEG-09-Systemd-Managed-Application.png)

### Reboot Persistence

SEG-10 shows expected interface addresses, UFW rules, an active/enabled service, listener and HTTP 200 for the earlier application. The newly located AUTH-01 journal capture includes a boot boundary and subsequent service start/HTTP 200 entries, corroborating that chronology. Paired boot checks and remote allow/deny retests for the authenticated application still remain missing. SEG-08 is a host-local test. Service recovery in REC-01 through REC-03 is a stop/start exercise, not proof of reboot persistence.

Evidence:

- [SEG-10 - Reboot Persistence Validation](Evidence/Segmentation/SEG-10-Reboot-Persistence-Validation.png)
- [SEG-12 - Final Segmentation Validation](Evidence/Segmentation/SEG-12-Security-Incident-Investigation.png)

## Evidence Index

The segmentation evidence index documents each screenshot, the control tested, expected result, and actual result:

- [Segmentation Evidence Index](Evidence/Segmentation/INDEX.md)
- [Authentication Evidence Index](Evidence/Authentication/INDEX.md)
- [Recovery Evidence Index](Evidence/Recovery/INDEX.md)
- [Primary Screenshot Set - 22 Captures](Evidence/PRIMARY-INDEX.md)
- [Archived Supporting Evidence](Evidence/Archive/INDEX.md)
- [Technical Report - Current Evidence Review](Reports/Final-Technical-Report.md)
- [Readiness Checklist](Reports/Readiness-Checklist.md)
- [Publication Review and Screenshot Holds](Reports/Publication-Review.md)

The SHA-256 evidence inventory is stored here:

- [Evidence SHA-256 Inventory](Evidence/evidence-sha256-inventory.csv)
- [Reviewed SHA-256 Inventory Including Supplemental Captures](Evidence/evidence-sha256-reviewed-20261010.csv)
- [Segmentation Source Provenance](Evidence/Segmentation/source-provenance.csv)
- [Authentication and Recovery Source Provenance](Evidence/Authentication-Recovery-Provenance.csv)
- [Current Image Inventory with Final Filenames](Evidence/evidence-sha256-final-names.csv)
- [Filename Change Map](Evidence/filename-change-map.csv)
- [Evidence Review and Proposed Changes](Reports/Segmentation-Review.md)

The controls demonstrate host-based microsegmentation and local application authentication. Full enterprise Zero Trust, device assurance, IPv6 coverage, outbound filtering and routed-network isolation remain outside the demonstrated scope.

## Authentication and Service Recovery

Ubuntu and Kali valid-credential Python requests return HTTP 200 and verify expected application content. Ubuntu unauthenticated requests and Kali invalid requests return 401. Windows remains blocked at TCP 8080 and never reaches authentication. The missing dedicated pre-recovery Kali unauthenticated screenshot is recorded rather than inferred from another test.

The controlled stop produces an inactive service, no listener and curl exit 7. Restart restores the listener and a local 401 challenge. Kali post-recovery no/invalid credential requests return 401 and ping succeeds twice. Valid credentials and application content have not been screenshot-verified after recovery.

These layers reduce opportunistic application access and show restoration without bypassing the authentication challenge. They do not demonstrate confidentiality in transit or recovery of authenticated user access. HTTP Basic without TLS remains an open risk.

## Open Finding

The DNS domain-root lookup discrepancy remains open. The latest detailed Kali capture returns `NOERROR` with `ANSWER: 0` for the domain-root A query, while the DC and global-catalog names return `10.10.10.10`. Earlier captures show unwanted NAT records and delegation errors. The latest frame does not prove those earlier unwanted records still exist; it also does not establish the intended domain-root answer or a completed DNS fix.

- [Open Findings](Reports/Open-Findings.md)
- [Security Findings Register](Findings/REGISTER.md)

## Missing or Follow-Up Evidence

- [Missing Evidence](Reports/Missing-Evidence.md)

## Repository Structure

```text
SECX-001/
  README.md
  Architecture/
    Architecture.md
  Evidence/
    INDEX.md
    PRIMARY-INDEX.md
    DNS/
      INDEX.md
    Authentication/
      INDEX.md
      AUTH-01 through AUTH-05 screenshots
    Segmentation/
      INDEX.md
      source-provenance.csv
      Supplemental/
      SEG-01-Baseline-Kali-to-Ubuntu.png
      SEG-02-Unrestricted-Application-Access.png
      SEG-03-Firewall-Enforcement.png
      SEG-04-Authorized-SSH-Access.png
      SEG-05-Least-Privilege-Application-Access.png
      SEG-06-UFW-Policy-and-Application-Listener.png
      SEG-07-Unauthorized-Client-Blocked.png
      SEG-08-Persistent-Network-Configuration.png
      SEG-09-Systemd-Managed-Application.png
      SEG-10-Reboot-Persistence-Validation.png
      SEG-11-Firewall-Blocked-Traffic-Logs.png
      SEG-12-Security-Incident-Investigation.png
    Monitoring/
      INDEX.md
    Recovery/
      INDEX.md
      REC-01-Controlled-Application-Outage.png
      REC-02-Service-Restored-Authentication-Enforced.png
      REC-03-Remote-Post-Recovery-Validation.png
    Archive/
      INDEX.md
      Authentication/
        TRB-01-Authentication-Startup-HTTP-000.png
        TRB-02-Ubuntu-Initial-Interactive-Tests-HTTP-401.png
      Recovery/
        REC-01-Pre-DNS-VirtualBox-Snapshot-Created.png
    evidence-sha256-inventory.csv
  Findings/
    REGISTER.md
  Reports/
    Duplicate-Evidence-Review.md
    Missing-Evidence.md
    Open-Findings.md
    Segmentation-Review.md
    Organization-Review.md
    Final-Technical-Report.md
    Readiness-Checklist.md
  Scripts/
    README.md
```

Existing root-level baseline images and prior hash inventories are retained. Monitoring and recovery indexes link to existing evidence where practical instead of duplicating screenshots.

The primary set contains 5 authentication, 3 service-recovery, 12 segmentation and 2 selected DNS captures. Earlier DNS, baseline and supplemental evidence remain available. Archived tests retain their actual outcomes; removal from the primary set does not turn a failed attempt into a success. Historical inventories reflect earlier filenames; use Evidence/filename-change-map.csv and the current naming inventory for path reconciliation.

## Troubleshooting

- The initial deny stage filtered both SSH and the application from Kali. The later application-approved stage kept SSH filtered while allowing HTTP; these are separate stages, not contradictory results.
- The first systemd listener query returned no row despite active/enabled status and HTTP 200. A later capture shows the listener; the immediate blank result remains documented.
- A local HTTP response confirms the application is serving locally. Remote client tests are required to validate segmentation.
- The source labeled AUTH-01 showed HTTP 000 during startup, not an authentication denial. The later unnamed capture supplies the verified local 401; both are preserved with accurate labels.
- An initial interactive request labeled correct credentials returned 401. A later Python getpass request returned 200 and verified content; the cause of the earlier failure is unconfirmed.
- DNS validation must distinguish the DC hostname, domain-root A records and AD locator records. Successful hostname resolution alone does not close the domain-root discrepancy.

## Lessons Learned

- Source-, interface- and port-specific rules make the application's allowed path easier to review.
- Failed client tests are stronger when paired with matching firewall logs and evidence that the service remains available to the approved client.
- Enabled services and correct runtime addresses support persistence preparation; boot evidence and remote tests demonstrate persistence across a reboot.
- Snapshots and configuration exports provide recovery preparation. A tested restore is separate evidence.
- A screenshot's filename is not a test result. Evidence descriptions must follow its visible output.
- A restored listener and 401 challenge prove availability of the authentication endpoint, not successful authenticated access after recovery.

## Publication Status

This folder is not currently a Git repository. No associated SECX/SecurityX repository was found in the accessible KSWISHA9 repository listing. Repository initialization, staging, commits and pushes are pending review. See the [organization review](Reports/Organization-Review.md) for discovery scope and publication checks.

All 54 retained screenshots have now been visually reviewed. AUTH-03, REC-03 and archived TRB-02 contain rejected test-password literals and are held from publication until password-free copies are reviewed. No valid secret was observed, but these images do not meet the requested no-passwords publication rule. See the [publication review](Reports/Publication-Review.md) and [per-image manifest](Evidence/Publication-Review.csv). Originals remain unchanged, and no publishing approval is implied.

The architecture documentation includes network trust boundaries, application access decisions and controlled recovery diagrams. The findings register supplies remediation priorities and closure evidence; DNS remains open, and valid credentials were not screenshot-verified after recovery.

## Safety Notes

- Original screenshots were preserved in their source locations.
- Evidence was copied into the project instead of moved.
- No VirtualBox network settings, firewall rules, or running VMs were changed during repository organization.
- No credentials, private keys, or secrets were intentionally added to the repository.
