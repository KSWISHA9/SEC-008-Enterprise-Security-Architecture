# Second Package Evidence Integration

**Received, reviewed and reconciled.** The ZIP contains 12 screenshots. All are already present by SHA-256; two additional manifest references point to existing TLS images omitted from the ZIP. Existing accurate paths are reused, so the project still contains 73 images.

| Package image / reference | Existing evidence | Actual result / handling |
| --- | --- | --- |
| TLS-01 | [Managed HTTPS activation](../../TLS/TLS-01-HTTPS-Service-Activation.png) | Active 8443 listener and CA-verified 401; no 8080 listener row |
| TLS-02 CA-Certificate-Chain | [Certificate validation](../../TLS/TLS-02-Lab-CA-and-Server-Certificate-Validation.png) | Chain OK; lab CA issuer and IP SAN 10.10.10.101; key-storage safety not proved |
| TLS-03, reference only | [Remote HTTPS challenge](../../TLS/TLS-03-Remote-HTTPS-Authentication-Challenge.png) | Trusted-CA 401/exit 0, not authenticated 200; absent from ZIP as declared |
| TLS-04, reference only | [HTTP retirement](../../TLS/TLS-04-HTTP-Retired-HTTPS-Enforced.png) | 8080 rule removed; 8443 listener/401; absent from ZIP as declared |
| REC-01 | [Controlled outage](../../Recovery/REC-01-Controlled-Application-Outage.png) | Historical 8080 service inactive, no listener, HTTP 000/curl 7 |
| REC-02 | [Service restored](../../Recovery/REC-02-Service-Restored-Authentication-Enforced.png) | Historical 8080 active/listener/401 |
| REC-03 | [Remote negative tests](../../Recovery/REC-03-Remote-Post-Recovery-Validation.png) | Historical no/invalid credentials 401, ping 2/2; supporting negative-test evidence; publication approval pending |
| AUTH-01, mislabeled | [Archived startup diagnostic](../../Archive/Authentication/TRB-01-Authentication-Startup-HTTP-000.png) | HTTP 000 and blank listener query, not the manifest's claimed 401; do not overwrite primary AUTH-01 |
| AUTH-02 | [Valid local Basic](../../Authentication/AUTH-02-Valid-Credentials-HTTP-200.png) | Historical Python 200/content verified; no valid password value printed |
| AUTH-03 | [Invalid remote Basic](../../Authentication/AUTH-03-Kali-Unauthorized-Requests.png) | Historical invalid credentials 401 only; supporting negative-test evidence; publication approval pending |
| AUTH-04 | [Valid remote Basic](../../Authentication/AUTH-04-Kali-Authorized-Access.png) | Historical Python 200/content verified; no valid password value printed |
| AUTH-05 | [Windows TCP denial](../../Authentication/AUTH-05-Unauthorized-Windows-Client-Blocked.png) | Source 10.10.10.1 TCP 8080 false/ping true, not new-port denial |
| Supplemental curl diagnostic | [Archived interactive tests](../../Archive/Authentication/TRB-02-Ubuntu-Initial-Interactive-Tests-HTTP-401.png) | Missing/invalid and purported-correct attempts all 401; supporting negative-test evidence; publication approval pending |
| Supplemental service-log diagnostic | [Primary AUTH-01](../../Authentication/AUTH-01-Unauthenticated-Access-Denied.png) | Actual local 401, listener and journal boot chronology; retained as primary evidence |

These matches confirm packaging provenance, not new independent tests or current deployment state. Historical Basic/HTTP and recovery are on 8080; later managed HTTPS Basic is 8443; Keycloak is 8444; the separate manual OIDC test app is 8445. No cutover, OIDC persistence, authenticated post-recovery access or current-stack restore is proved by this package.

All 12 included images were visually inspected. The current owner-approved decision retains the three rejected-password images in supporting/archive roles; no valid password, raw token, secret or private-key value was observed. No pixels were changed, originals deleted, scripts executed or files published.

- [Original manifest](Provided-Manifest.csv): unchanged source descriptions, including the inaccurate AUTH-01 claim.
- [Original README as text](Provided-README.md.txt): preserved package commentary, not maintainer instructions.
- [Manifest/archive/project reconciliation](Reconciliation.csv): included-entry hashes and declared omitted references.
- [ZIP provenance](Package-Provenance.json): archive identity, SHA-256 and counts.
- [Project package review](../../../Reports/Package-Reconciliation.md)
