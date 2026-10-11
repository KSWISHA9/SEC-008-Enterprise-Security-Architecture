# Recovery Evidence Index

Stage note: REC-01/02/03 cover the earlier 8080 Basic application. They do not validate recovery of the 8443 HTTPS, 8444 Keycloak or manual 8445 OIDC deployment. See [current validation](../../Reports/Test-Validation-Report.md).

Publication note: REC-03 is retained as supporting historical recovery evidence under the owner-approved [Publication Review](../../Reports/Publication-Review.md). It shows rejected credentials, not a confirmed working password or positive authenticated access after recovery. Publication approval remains pending.

| Evidence | Control Tested | Expected Result | Actual Result |
| --- | --- | --- | --- |
| [REC-01 controlled outage](REC-01-Controlled-Application-Outage.png) | Controlled loss of application availability | Stopped service becomes unavailable | Service inactive; no listener row on 8080; HTTP 000 and curl exit code 7. HTTP 000 is a curl no-response placeholder, not an HTTP server response |
| [REC-02 service restoration](REC-02-Service-Restored-Authentication-Enforced.png) | Restore service while retaining authentication | Active listener; unauthenticated request rejected | Service active; Python listener at 10.10.10.101:8080; unauthenticated HTTP 401 |
| [REC-03 remote negative tests](REC-03-Remote-Post-Recovery-Validation.png) | Remote connectivity and authentication after restart | No/invalid credentials rejected; host reachable | Kali receives 401 without credentials and 401 with invalid credentials; ping 2/2 successful. No valid-credential test in this frame |

## Supporting Preparation Evidence

| Evidence | Control Tested | Expected Result | Actual Result |
| --- | --- | --- | --- |
| [Legacy DNS snapshot](../Archive/Recovery/REC-01-Pre-DNS-VirtualBox-Snapshot-Created.png) | Pre-change rollback preparation | Snapshot creation completes and snapshot appears in listing | VBoxManage snapshot command completes at 100%; snapshot UUID and SECX-001-Pre-DNS-Remediation name appear in listing |
| [DNS-14](../DNS/DNS-14-Pre-Remediation-Snapshot.png) | DNS/registry export preparation | Export operations complete | Commands and success messages visible; no backup artifact inspection or restore validation |

The legacy snapshot image was copied unchanged from `Pictures/Screenshots/SECX-001-Pre-DNS-Remediationn.png` and is now retained in Archive/Recovery. It keeps its existing filename, including the earlier REC-01 prefix. Primary REC-01 denotes the controlled application outage. The similarly named source without the extra `n` only shows OV-DC01 powered off and was not labeled as a snapshot test.

No VM/service command was executed during organization. Screenshots document the operator's earlier lab actions. Service restart and negative authentication checks are verified; backup/snapshot restore, measured recovery timing, valid credentials/content after recovery, and fresh post-recovery Windows TCP denial remain missing. This recovery was a service stop/start, not a backup restoration or a reboot test.

New recovery captures were visually reviewed: no valid secret value is displayed. REC-03 includes an explicit rejected test-password literal. Original pixels are preserved; source/duplicate hashes are recorded in [Authentication-Recovery-Provenance.csv](../Authentication-Recovery-Provenance.csv).

The [second package](../Packages/Additional-Authentication-Recovery-TLS/INDEX.md) contains exact matches to REC-01/02/03. Reusing them adds provenance, not fresh recovery tests or closure of the positive-access/current-stack validation gaps.
