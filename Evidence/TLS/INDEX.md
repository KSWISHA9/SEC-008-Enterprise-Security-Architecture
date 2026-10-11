# TLS Evidence Index

Five unique screenshots; source filenames and hashes are in [provenance](../TLS-IAM-Provenance.csv). Images were inspected before assignment. No valid credential or private-key value was visible.

| ID / screenshot | Control tested | Expected | Actual and limit |
| --- | --- | --- | --- |
| [TLS-01](TLS-01-HTTPS-Service-Activation.png) | Managed app HTTPS activation | Active service/listener; trusted-CA response; old HTTP listener absent | Active; 10.10.10.101:8443 listener; CA-verified 401; 8080 query has no listener row. Not an authenticated-content test |
| [TLS-02](TLS-02-Lab-CA-and-Server-Certificate-Validation.png) | Lab certificate chain/identity | Certificate verifies and matches target identity | server.crt: OK; issuer OmniVerse SEC-008 Lab CA; IP SAN 10.10.10.101; validity shown. Generation progress is not key material or proof of secure CA storage |
| [TLS-03](TLS-03-Remote-HTTPS-Authentication-Challenge.png) | Kali trusted-CA HTTPS reachability | Certificate-verified endpoint answers with challenge without credentials | HTTP 401 and curl exit 0 using --cacert. Original source says Authenticated HTTPS Access, but no authenticated 200 is shown |
| [TLS-04](TLS-04-HTTP-Retired-HTTPS-Enforced.png) | Retire HTTP access rule/listener | 8080 rule absent; 8443 remains available | Rule deleted; 8443 approved-source rule and listener; no 8080 listener; CA-verified local 401 |
| [TLS-05](TLS-05-Remote-HTTP-Retirement-Validation.png) | Remote port and HTTPS comparison | 8443 available; old HTTP unavailable | Kali scan: 22/8080 filtered, 8443 open; CA-verified 401; HTTP 8080 result 000. No Windows new-port denial test |

These tests establish transport validation for the captured 8443 path, not comprehensive TLS version/cipher policy, mutual TLS, certificate revocation, 8445 browser trust or authenticated Basic content over HTTPS. The 8443 service remains Basic; OIDC is a separate test deployment.

The [second package](../Packages/Additional-Authentication-Recovery-TLS/INDEX.md) supplies exact TLS-01/02 matches and references the already retained TLS-03/04 hashes. Its CA-Certificate-Chain filename maps to the existing TLS-02 name. No duplicate copy or new TLS validation claim was added.
