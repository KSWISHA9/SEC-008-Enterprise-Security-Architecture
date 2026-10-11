# Authentication Evidence Index

Stage note: these are earlier Basic/HTTP 8080 results, not current OIDC tests. See the [TLS index](../TLS/INDEX.md) and [IAM index](../IAM/INDEX.md) for later work. Historical plaintext observations do not assert that 8080 is still active.

Publication note: AUTH-03 is retained as supporting evidence of rejected credentials (HTTP 401), under the owner-approved decision in [Publication Review](../../Reports/Publication-Review.md). No working credential is visible; reuse elsewhere is not independently checked. Originals are unchanged and publication approval remains pending.

This historical phase demonstrates local HTTP Basic application authentication behind host-based microsegmentation. These captured requests used HTTP without TLS; credentials and content were not protected in transit at that stage. Later TLS evidence documents retirement of 8080 and HTTPS on 8443. None of these Basic tests is an OIDC test or proof of full enterprise Zero Trust.

| ID | Screenshot | Control Tested | Expected Result | Actual Result |
| --- | --- | --- | --- | --- |
| AUTH-01 | [Ubuntu unauthenticated request](AUTH-01-Unauthenticated-Access-Denied.png) | Local application authentication challenge | Request without credentials returns 401 | HTTP/1.0 401 Unauthorized and WWW-Authenticate: Basic visible; listener bound to 10.10.10.101:8080; journal also shows a 401 request |
| AUTH-02 | [Ubuntu valid credentials](AUTH-02-Valid-Credentials-HTTP-200.png) | Valid local authentication and content check | Valid credentials return 200 and expected content | Python getpass-based request returns AUTH RESULT: 200 and CONTENT VERIFIED: True; the password and constructed authorization value are not printed |
| AUTH-03 | [Kali invalid credentials](AUTH-03-Kali-Unauthorized-Requests.png) | Remote invalid-credential rejection | Incorrect credentials return 401 | One incorrect-credential curl test returns HTTP 401. No unauthenticated test appears in this frame |
| AUTH-04 | [Kali valid credentials](AUTH-04-Kali-Authorized-Access.png) | Remote valid authentication and content check | Valid credentials return 200 and expected content | Python getpass-based request returns REMOTE AUTHENTICATION: 200 and APPLICATION VERIFIED: True; valid password not displayed |
| AUTH-05 | [Windows TCP denial](AUTH-05-Unauthorized-Windows-Client-Blocked.png) | Network boundary before application authentication | Windows source cannot reach application TCP port | Source 10.10.10.1 to 10.10.10.101:8080: ping succeeds, TcpTestSucceeded False; HTTP/authentication is not reached |

Kali unauthenticated access before recovery is reported by the lab operator, but a dedicated pre-recovery screenshot was not located. [REC-03](../Recovery/REC-03-Remote-Post-Recovery-Validation.png) independently verifies a Kali unauthenticated 401 after recovery. Do not assign a pre-recovery timestamp to that test.

## Troubleshooting Captures

- [Initial startup check](../Archive/Authentication/TRB-01-Authentication-Startup-HTTP-000.png): the source was named AUTH-01-Unauthenticated-Access-Denied.png, but actually shows active service, no listener row and HTTP 000. It is not a successful 401 test. Curl exit status is not shown here.
- [Initial interactive tests](../Archive/Authentication/TRB-02-Ubuntu-Initial-Interactive-Tests-HTTP-401.png): unauthenticated and incorrect requests return 401; an attempted correct-credential curl request also returns 401. The reason is not established. AUTH-02 separately verifies a successful Python request; do not claim every attempt succeeded.

## Visual Secrets Review

All images in this folder were inspected before import. Successful requests use hidden password prompts. Code references to password variables and Basic encoding are visible, but neither valid password values nor encoded Authorization header values are displayed. AUTH-03 and the troubleshooting capture contain explicit rejected test-password literals; these are not evidence of a valid secret and are not reproduced in documentation.

Original pixels were preserved and SHA-256 checked. Source names, duplicate captures and full hashes are in [Authentication-Recovery-Provenance.csv](../Authentication-Recovery-Provenance.csv). No authentication configuration file was imported.

The [second package](../Packages/Additional-Authentication-Recovery-TLS/INDEX.md) reuses these existing images. Its AUTH-01 filename instead refers to archived TRB-01/HTTP 000; its supplemental service-log diagnostic matches the valid primary AUTH-01/401. The package labels were not allowed to overwrite the accurately indexed screenshots.
