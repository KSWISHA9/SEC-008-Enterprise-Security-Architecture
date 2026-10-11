# Primary Screenshot Set

Twenty-two selected captures using the requested filenames. These cover successful validations, including deliberate rejection/outage tests; they do not establish that the overall project is complete. DNS remains open and HTTP Basic lacks TLS. Detailed indexes retain expected/actual results and evidence limitations.

| ID | Screenshot | Visible Result |
| --- | --- | --- |
| AUTH-01 | [Unauthenticated access denied](Authentication/AUTH-01-Unauthenticated-Access-Denied.png) | Ubuntu HTTP 401 diagnostic |
| AUTH-02 | [Valid credentials HTTP 200](Authentication/AUTH-02-Valid-Credentials-HTTP-200.png) | Ubuntu Python 200; content check true |
| AUTH-03 | [Kali unauthorized requests](Authentication/AUTH-03-Kali-Unauthorized-Requests.png) | Invalid credentials 401 only |
| AUTH-04 | [Kali authorized access](Authentication/AUTH-04-Kali-Authorized-Access.png) | Kali Python 200; content check true |
| AUTH-05 | [Unauthorized Windows client blocked](Authentication/AUTH-05-Unauthorized-Windows-Client-Blocked.png) | TCP 8080 fails from 10.10.10.1 |
| REC-01 | [Controlled application outage](Recovery/REC-01-Controlled-Application-Outage.png) | Inactive; no listener; HTTP 000; curl exit 7 |
| REC-02 | [Service restored, authentication enforced](Recovery/REC-02-Service-Restored-Authentication-Enforced.png) | Active; listener restored; unauthenticated 401 |
| REC-03 | [Remote post-recovery validation](Recovery/REC-03-Remote-Post-Recovery-Validation.png) | Kali no/invalid credentials 401; ping 2/2; no valid-credential retest |
| SEG-01 | [Baseline Kali to Ubuntu](Segmentation/SEG-01-Baseline-Kali-to-Ubuntu.png) | Ping 3/3; SSH open; 80/443 closed |
| SEG-02 | [Unrestricted application access](Segmentation/SEG-02-Unrestricted-Application-Access.png) | 22/8080 open; HTTP 200 before restrictive rules |
| SEG-03 | [Firewall enforcement](Segmentation/SEG-03-Firewall-Enforcement.png) | 22/8080 filtered; HTTP timeout |
| SEG-04 | [Authorized SSH access](Segmentation/SEG-04-Authorized-SSH-Access.png) | Windows reaches Ubuntu shell |
| SEG-05 | [Least-privilege application access](Segmentation/SEG-05-Least-Privilege-Application-Access.png) | Kali 8080 open, HTTP 200; 22 filtered |
| SEG-06 | [UFW policy and application listener](Segmentation/SEG-06-UFW-Policy-and-Application-Listener.png) | Active UFW; scoped allows; listener visible |
| SEG-07 | [Unauthorized client blocked](Segmentation/SEG-07-Unauthorized-Client-Blocked.png) | Windows TCP 8080 fails; ping succeeds |
| SEG-08 | [Persistent network configuration](Segmentation/SEG-08-Persistent-Network-Configuration.png) | Runtime IP/rules/local HTTP check; stored persistence config not shown |
| SEG-09 | [Systemd-managed application](Segmentation/SEG-09-Systemd-Managed-Application.png) | Active/enabled; local HTTP 200; immediate listener table blank |
| SEG-10 | [Reboot persistence validation](Segmentation/SEG-10-Reboot-Persistence-Validation.png) | Reported post-reboot state; current authenticated remote persistence tests incomplete |
| SEG-11 | [Firewall blocked traffic logs](Segmentation/SEG-11-Firewall-Blocked-Traffic-Logs.png) | UFW BLOCK source/target/port details |
| SEG-12 | [Security incident investigation](Segmentation/SEG-12-Security-Incident-Investigation.png) | Correlated blocks, UFW policy, service state; no full incident lifecycle claim |
| DNS-20 | [Controlled DNS cleanup](DNS/DNS-20-Controlled-DNS-Cleanup.png) | Cleanup output and DCDIAG exit 0; long-term registration source explicitly unconfirmed |
| DNS-21 | [Kali residual finding](DNS/DNS-21-Kali-Client-Residual-Finding.png) | Domain-root A query NOERROR with no answer; DC/GC resolve; finding open |

The detailed [authentication](Authentication/INDEX.md), [recovery](Recovery/INDEX.md), [segmentation](Segmentation/INDEX.md) and [DNS](DNS/INDEX.md) indexes remain authoritative for scope and chronology. [Archived attempts](Archive/INDEX.md), earlier DNS captures, baseline images and segmentation supplemental evidence are retained separately.
