# Before-and-After Security Comparison

Stages are chronological lab evidence, not a single completed production migration.

| Surface | Initial / earlier stage | Current demonstrated or reported stage | Still unfinished |
| --- | --- | --- | --- |
| Exposure | 22/8080 broadly reachable from Kali | Source/port-specific UFW rules; historical Windows denial | Windows/other-path tests for new ports |
| Transport | Basic over plaintext HTTP 8080 | CA-verified HTTPS 8443; HTTP retired | 8445 dedicated TLS checks, crypto/trust lifecycle |
| Identity | Local Basic account | Keycloak confidential OIDC test integration on 8445 | 8443 cutover; comprehensive validation review |
| Authorization | Basic credential permits content | Analyst dashboard allowed; authenticated viewer denied | Wider route coverage, role-change/replay/session tests |
| Operations | Manual baseline then systemd Basic service | Managed 8443 Basic; active Keycloak; manually launched OIDC test | Managed OIDC and reboot/current-stack recovery |
| Logging | Baseline/no correlated denial story | Local UFW events correlated with negative tests | Identity/app audit policy and retention review |
| Recovery | Snapshot/export preparation | Earlier 8080 controlled stop/restart | Current-stack positive access, actual restore, timing |
| DNS | Unwanted records/delegation investigation | DC/GC address success; domain-root empty A answer | Intended root behavior and stable closure |
| Trust management | Local lab setup | Private lab CA in use | Signing key separation; permanent admin model |

The current state is stronger than the initial lab, but Basic still runs on 8443 and OIDC remains a separate manual test app. Avoid describing this as a completed replacement or enterprise Zero Trust rollout. See [test results](Test-Validation-Report.md).
