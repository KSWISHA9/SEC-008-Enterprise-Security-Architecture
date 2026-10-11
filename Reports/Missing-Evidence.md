# Missing Evidence and Coverage Gaps

TLS and IAM screenshots now exist. Missing items below are not invented, and their absence is not proof a control failed.

| Evidence needed | Current basis | Why still needed |
| --- | --- | --- |
| Analyst username, test port and dashboard in one safe capture | Authorized dashboard only; ov-analyst/8445 operator-confirmed | Correlates identity, endpoint and authorization |
| Numeric viewer HTTP 403 and requested analyst route | Signed-in viewer and Forbidden page; 403 operator-confirmed | Confirms protocol status and route without exposing cookies |
| Sanitized 8445 client callback/settings and listener | Test-registration message and firewall rule; initial URI is 8443 | Verifies actual test endpoint and callback |
| Authorization-code exchange / token-validation summary | Standard-flow client and successful UI; implementation operator-reported | Shows relevant checks without raw token/code/secret |
| Invalid signature/issuer/audience/expiry, replay and state/nonce tests | Not located | Defines adversarial validation coverage |
| Session expiration, secure cookies, logout/refresh and role-change behavior | Viewer roles displayed; logout link only | Confirms session and role lifecycle |
| Trusted-CA certificate identity/chain tests for 8445 | Certificate/HTTPS tests for 8443 and IdP discovery for 8444 | Browser UI alone does not establish test-app TLS validation |
| Authenticated HTTPS Basic 200 on 8443 | TLS-03 mislabeled source actually shows 401 | Verifies content over the managed HTTPS path |
| OIDC 8443 cutover and rollback | Explicitly not completed | Needed before claiming replacement of Basic service |
| Managed OIDC unit, restart and paired reboot tests | Manual test launch reported | Needed for current OIDC operational persistence |
| Current HTTPS/Keycloak/OIDC recovery and positive/negative retests | Historical recovery covers 8080 Basic only | Does not establish recovery of current stack |
| Windows denial on 8443, 8444 and 8445 | Historical Windows denial only on 8080; new allow rules visible | Configured rules are not fresh end-to-end negative tests |
| Sanitized stored network/UFW/unit configuration and Kali IP | Runtime network and configured allowlist present | Confirms stored state and source inventory |
| CA key separation and safe secret-storage review | Co-location operator-reported | Do not export private keys or secret values as evidence |
| Permanent admin model | Temporary-admin warning visible | Closure screenshot/configuration summary after operator work |
| DNS final validation | Domain-root empty A answer remains open | Establish intended behavior and repeat tests |
| Backup restore and RTO/RPO measurements | Snapshot/export preparation and old stop/start only | Separate restore evidence required before recovery claims |

There is no need to publish raw tokens, cookie headers, private keys or credentials. Use safe summaries, suppressed output and redacted configuration structure. Existing images must not be altered to manufacture a missing test. Historical pre-recovery Kali unauthenticated and positive post-recovery Basic evidence gaps remain documented in preserved reports.
