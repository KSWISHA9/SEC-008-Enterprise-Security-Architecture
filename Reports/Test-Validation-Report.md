# Test and Validation Report

Review date: 2026-10-10. Tests were performed by the operator, not rerun by repository maintenance. Configuration, output, UI and operator testimony are separate evidence types. Numeric statuses are recorded only when visible or explicitly attributed.

| Test / stage | Expected | Actual | Evidence / qualification |
| --- | --- | --- | --- |
| Baseline | Identify exposure | Ping 3/3; later 22/8080 open, HTTP 200 | SEG-01/02 |
| Initial UFW enforcement | Deny unapproved paths | 22/8080 filtered; timeout | SEG-03 |
| Approved app path | Permit app, not lab-path SSH | 8080 open/200; 22 filtered | SEG-05 |
| Windows negative | Deny TCP app access | TCP false; ping true | SEG-07/AUTH-05, earlier 8080 stage |
| Logging/investigation | Correlate denial | UFW BLOCK source 10.10.10.1 to 10.10.10.101:8080 | SEG-11/12 |
| Stored network persistence | Configuration survives boot | Runtime IPs and old boot chronology present; config export absent | SEG-08/10, AUTH-01; persistence operator-reported |
| Ubuntu Basic | Reject missing; accept valid | 401; Python 200/content true | AUTH-01/02, earlier HTTP |
| Kali Basic | Reject invalid; accept valid | 401; Python 200/content true | AUTH-03/04, earlier HTTP |
| Earlier outage | Remove listener/service response | Inactive; no listener; HTTP 000/curl 7 | REC-01 |
| Earlier restart | Restore authentication endpoint | Active/listener; 401 | REC-02 |
| Earlier remote recovery | Negative requests still denied | No/invalid credentials 401; ping 2/2 | REC-03; positive post-recovery test missing |
| Certificate identity | Chain trusted; IP SAN matches | server.crt OK; IP SAN 10.10.10.101 | TLS-02 |
| Managed HTTPS | Trusted-CA endpoint answers | Active 8443 listener; local 401; Kali 401/exit 0 | TLS-01/03 |
| HTTP retirement | 8080 unavailable, HTTPS retained | Old allow removed; only 8443 listener; remote 8080 filtered/000 | TLS-04/05; not proof all paths tested |
| Keycloak availability | Version/service/HTTPS ready for lab test | 26.8.0; active; 8444; discovery 200 | IAM-06/07/08 |
| Realm/role/client | Distinct roles, confidential standard flow | Roles visible; publicClient false; standardFlow true | IAM-01/02/03/05 |
| OIDC test endpoint | Isolated 8445 callback/listener | Rule/message visible; port and manual launch operator-confirmed | IAM-09; final URI/listener absent |
| Analyst login/RBAC | Named analyst accesses restricted route | Authorized dashboard | IAM-11; ov-analyst/8445 operator-confirmed, status not captured |
| Viewer login | Authenticate and retrieve roles | ov-viewer and realm roles displayed | IAM-12 |
| Viewer RBAC denial | Authenticated viewer cannot access analyst route | Forbidden page; HTTP 403 operator-confirmed | IAM-13; numeric status/request route absent |
| Token validation | Accept correct token, reject invalid variants | Operator reports validation; no adversarial test/source evidence | Do not certify complete validation from roles/UI |
| Production-port cutover | OIDC replaces 8443 Basic | Not completed | Explicit operator limitation |
| OIDC persistence/current-stack recovery | Survive restart/reboot and restore access | Not validated | Old 8080 recovery cannot substitute |

## Interpretation Rules

HTTP 401 shows a reachable authentication challenge, not successful login. HTTP 403/Forbidden denotes an authorization denial in the reported viewer test, not invalid credentials. Curl HTTP 000 means no HTTP response was obtained. A filtered port is a remote scan observation, not evidence of cryptographic protection. Displayed roles do not establish signature/claim checking.

Client settings prove configuration at that captured stage, not the current final callback. No authorization code, token, cookie or secret should be published to prove a flow. Dedicated safe summaries and negative tests are better evidence.

See [TLS](../Evidence/TLS/INDEX.md), [IAM](../Evidence/IAM/INDEX.md), [missing evidence](Missing-Evidence.md) and [readiness](Readiness-Checklist.md).
