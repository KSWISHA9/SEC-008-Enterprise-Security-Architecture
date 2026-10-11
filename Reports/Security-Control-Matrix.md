# Security Control Matrix

Mappings below are educational relevance mappings, **not implementation of every control requirement, a baseline assessment or compliance certification**. Demonstrated behavior and missing coverage are described separately.

| Lab behavior | Evidence status | Relevant NIST SP 800-53 Rev. 5 concepts | Zero Trust relationship / limit |
| --- | --- | --- | --- |
| UFW source/interface/port rules | Earlier denial tests; updated rules captured | AC-4 Information Flow Enforcement; SC-7 Boundary Protection | Narrow workload path, not user/device trust |
| Analyst-only route and viewer denial | Distinct UI outcomes; test details qualified | AC-3 Access Enforcement; AC-3(7) Role-Based Access Control; AC-6 Least Privilege | Identity/role-informed access, limited route coverage |
| Keycloak lab accounts and OIDC client | Configuration/UI supported; protocol internals reported | IA-2 Identification and Authentication; IA-5 Authenticator Management | User identity layer; MFA/lifecycle/secret assurance not established |
| Trusted-CA HTTPS and HTTP retirement | 8443 certificate/remote tests captured | SC-8 Transmission Confidentiality and Integrity; SC-13 Cryptographic Protection | Protect tested transport, not full crypto-policy validation |
| CA signing-key placement | Co-location reported; separation unfinished | SC-12 Cryptographic Key Establishment and Management | Trust dependency needs operational isolation |
| UFW events and investigation | Local kernel events captured | AU-12 Audit Record Generation; AU-6 Audit Record Review, Analysis, and Reporting | Visibility, not continuous enterprise policy evaluation |
| Service stop/restart | Earlier 8080 exercise captured | CP-10 System Recovery and Reconstitution | Recovery concept; not backup restore/current-stack continuity |
| Network/service configuration | Runtime/old boot partial; manual OIDC incomplete | CM-6 Configuration Settings; CM-7 Least Functionality | Defined intended state; current reproducibility/persistence gaps |

The [NIST SP 800-53 Rev. 5 catalog](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-53r5.pdf) supplies these control identifiers/titles. The lab descriptions are project-specific relevance judgments, not assertions that all organization-defined parameters or enhancements are satisfied.

[NIST SP 800-207](https://csrc.nist.gov/pubs/sp/800/207/final) explains Zero Trust beyond network location. This lab demonstrates selected layered concepts; device posture, continuous evaluation, broad resource policy, mature key/secret governance and enterprise operations remain outside verified scope.

## Evidence-to-Control Review

A configured allowlist needs end-to-end tests; an identity provider needs reviewed application token/session enforcement; a recovery exercise needs scope and positive access checks. See [validation](Test-Validation-Report.md) and [findings](../Findings/REGISTER.md) before using a mapping in an interview or audit.

Both [supplied packages](Package-Reconciliation.md) map to existing images, not new control tests. The second ZIP's historical AUTH/REC evidence cannot be used to claim HTTPS/OIDC recovery or broader CP-10 coverage. A trusted-CA 401 supports the transport/endpoint test, not successful authentication; conceptual IA/AC mapping does not change that result. Its mislabeled AUTH-01 actually shows no HTTP response, so it adds no successful authentication-challenge evidence; the separate log/401 capture remains the verified source.
