# SEC-008 Consolidated Technical Report

Review date: 2026-10-10. **Documented lab milestone complete; operational promotion and further validation remain open.** Completion covers the demonstrated controls and evidence-backed case study, not production readiness or the full planned OIDC deployment. Previous documentation is retained in [History](History/INDEX.md).

## Executive Assessment

OmniVerse SEC-008 is a simulated security engineering lab built in VirtualBox using Windows Server AD/DNS, Ubuntu and Kali. It demonstrates layered protection of an internal training application: host firewall segmentation, access logging/investigation, local authentication, lab-CA HTTPS, HTTP retirement and a separate Keycloak OIDC/RBAC test.

The strongest current result is that a signed-in viewer is not entitled to the analyst dashboard. Authentication and authorization are distinct. The authorized dashboard and denied page are preserved as separate evidence. This project is suitable interview material with its limits disclosed, but is not a production deployment or complete enterprise Zero Trust system.

## Business Objective and Threat Model

Reduce unnecessary workload exposure, protect credentials/content in transit, enforce the intended role boundary and retain auditable test evidence. A business analogue would be controlling an internal analyst tool before promoting a change; no real client engagement, measured cost reduction, SLA, certification or production user count is claimed.

Threats include unauthorized network clients, authenticated users without the required role, interception of earlier HTTP credentials, token/session abuse, service unavailability, DNS discrepancies and compromise of the shared application/identity/CA host. See [architecture](../Architecture/Architecture.md) for boundaries and residual risk.

## Environment and Deployment State

Ubuntu 10.10.10.101 hosts the application paths. Kali is the configured approved source 10.10.10.100; Windows negative tests show source 10.10.10.1. OV-DC01 is the AD/DNS investigation host at 10.10.10.10. Ubuntu has enp0s8 lab networking and enp0s3 NAT-facing administration.

| Port | State / purpose | Confidence |
| --- | --- | --- |
| 8080 | Retired earlier HTTP app | Local absence of listener and remote filtered/no-response evidence |
| 8443 | Managed HTTPS Basic app | Active listener, trusted-CA 401 and UFW allow rule captured |
| 8444 | Keycloak 26.8.0 HTTPS IdP | Version/startup, active service, listener and remote discovery 200 captured |
| 8445 | Manual OIDC test app | Operator-confirmed test deployment; rule captured; browser screenshots do not show port |
| 22 | Administrative SSH on enp0s3 | Earlier successful shell and interface-scoped Anywhere rules; source restriction not established |

The OIDC app has not replaced the 8443 Basic service. Its manual launch has not been reboot-tested. Keycloak's displayed production runtime profile is a software startup mode, not proof that the lab is production-ready.

## Controls and Evolution

1. Baseline Nmap/curl tests showed broad SSH/application exposure.
2. UFW default-deny inbound policy and approved-source application rules narrowed the lab path. Windows TCP failure was correlated with kernel UFW BLOCK events.
3. Local Basic authentication rejected missing/invalid credentials and returned content for valid Ubuntu/Kali requests in the earlier HTTP phase.
4. A private lab CA signed a server certificate with IP SAN 10.10.10.101. Chain validation returned server.crt: OK. CA-verified local/Kali HTTPS requests reached the authentication endpoint.
5. The 8080 allow rule was removed. Local listener output shows 8443 only; a remote scan shows 8080 filtered and 8443 open. The HTTPS endpoint still returns 401 without credentials.
6. Keycloak realm roles and lab users were configured with a confidential OIDC standard-flow client. The operator completed authorization-code login in the separate test app.
7. The authorized dashboard, signed-in viewer roles and denied dashboard demonstrate distinct RBAC outcomes with the qualifications below.

Persistent Ubuntu networking is operator-reported; runtime IPs and historical boot chronology are captured, but the stored network configuration is not exported. Documentation does not promote this to verified OIDC reboot persistence.

## Authentication, Authorization and Token Evidence

IAM-03 shows a confidential OpenID Connect client with standard flow enabled and an initial 8443 callback. IAM-05 shows client authentication on and the same initial callback. IAM-09 shows an 8445 firewall allow and a test-callback registration message, not the URI itself.

The operator confirms ov-analyst authenticated on 8445 and accessed the analyst-only dashboard. IAM-11 shows the authorized dashboard but no username/port. IAM-12 independently displays ov-viewer signed in with viewer and default realm roles. IAM-13 shows a Forbidden page; the numeric HTTP 403 is operator-confirmed rather than visible in that cropped frame.

Token validation is reported by the operator. Displayed realm roles support application role retrieval, not comprehensive cryptographic validation. Source/configuration review, redacted validation summaries and negative cases for signature, issuer, audience, expiry and replay remain missing. Raw tokens, session cookies and client secrets must not be published.

## Validation Methodology

Review preserved screenshots; hash source/copy pairs; separate configuration evidence, runtime output, UI outcomes and operator statements. Compare positive and negative tests at each stage without treating a historical result as a fresh retest. No network probes, VM changes, firewall edits or application execution were performed by the portfolio maintainer.

The [test report](Test-Validation-Report.md) records expected/actual results and limits. The [evidence index](../Evidence/INDEX.md) provides individual screenshots. The [before/after comparison](Before-After-Comparison.md) explains stage transitions; the [control matrix](Security-Control-Matrix.md) provides conceptual NIST mappings.

## Recovery Interpretation

REC-01 shows controlled stop: inactive, no listener on 8080, curl HTTP 000 and exit 7. HTTP 000 is a client no-response placeholder, not a server status. REC-02 shows service restoration, listener and unauthenticated 401. REC-03 shows Kali no/invalid credentials returning 401 and ping 2/2.

These are earlier Basic/8080 recovery tests, not current HTTPS/OIDC recovery or backup restoration. Valid-credential access after that recovery, current-stack recovery, fresh Windows denial and measured RTO/RPO are not established. Successful pre-recovery authentication cannot substitute for them.

## Findings and Priorities

| Priority | Work | Closure needed |
| --- | --- | --- |
| P1 before promotion | OIDC cutover and managed operations | Approved change/rollback plan, 8443 identity/role tests, service restart and reboot validation |
| P1 before broader trust | CA separation and secret/session/token hardening | Separate signing key, reviewed secret handling and adversarial validation without exposing secrets |
| P2 | DNS-001, admin exposure, temporary administrator and path coverage | Intended DNS policy and repeat validation; management review; permanent admin model; updated-port negative tests |
| P2 | Recovery/restore coverage | Current-stack positive/negative retests; separate backup restore and timings if claiming objectives |

The [findings register](../Findings/REGISTER.md) distinguishes mitigation of earlier plaintext transport from current operational risks. DNS-001 stays open. CA private-key co-location is operator-reported, not verified by reading the key. No finding is closed solely by documentation.

## Lessons and Portfolio Positioning

Layered controls are useful only when their scope is clear: an IP allowlist is not identity assurance; HTTPS success is not authenticated success; a signed-in viewer is not authorized for every route; an enabled service is not proof a separate manual app survives reboot. See [lessons learned](Lessons-Learned.md) and [interview/resume material](Interview-and-Resume.md).

The project illustrates selected Zero Trust concepts, but lacks complete identity/device assurance, continuous policy evaluation, mature key/secret operations, high availability and production cutover. Conceptual mapping is not compliance certification.

## Evidence and Publication

19 unique TLS/IAM screenshots were imported as unchanged copies with accurate names. Four generic/named duplicate pairs were identified by SHA-256 and copied only once. Existing evidence and source files were not deleted or overwritten. July Keycloak/SSO evidence belongs to other portfolio work and was not imported into this lab.

The owner approved retaining AUTH-03/REC-03 as supporting evidence and TRB-02 as archived troubleshooting. Visible strings were rejected; no confirmed working password is exposed in those images. The blanket rejected-password hold is removed. New images show prompts or configuration metadata but no visible password, token, client/session secret or private-key value. See [publication review](Publication-Review.md), [provenance](../Evidence/TLS-IAM-Provenance.csv) and [current inventory](../Evidence/evidence-sha256-TLS-IAM-current.csv).

The project is documentation-ready for current results, publication approval and exact-release review remain pending. No Git metadata/matching repository was found in the current accessible scope. No staging, commit, push, VM change or secret-file import occurred.

## Supplied Manifest and Second-Package Intake

The newly supplied EVIDENCE-MANIFEST.csv contains 19 IAM/TLS rows and 17 unique hashes, all matched to existing project screenshots. Two repeated hashes refer to alternate names of images already retained. No image was re-imported or overwritten. Its inaccurate login-page label and imprecise TLS-authentication wording were checked against actual pixels rather than accepted as new claims.

That first manifest contains no Authentication or Recovery entries. The subsequently supplied second ZIP has now been inspected and integrated: all 12 included PNGs match its manifest and existing project evidence, and its two references to omitted TLS-03/04 images match the existing TLS files. No duplicate screenshot was imported and no original was overwritten. Its manifest and README are preserved as source material, not treated as executable instructions or unquestioned test results.

The second-package AUTH-01 is mislabeled: visible HTTP 000 with a blank listener query matches archived TRB-01, not a successful 401 challenge. Its supplemental log diagnostic is the actual primary AUTH-01 401 capture. The interactive curl diagnostic remains a failed purported-correct-credential test; later Python success does not retroactively change it. All 12 package images were visually reviewed; the three rejected-password images follow the current owner-approved supporting/archive dispositions.

Existing HTTP/Basic recovery remains historical; HTTPS on 8443 and OIDC testing on 8445 do not inherit those results. No positive post-recovery credential/content retest, fresh Windows new-port denial, OIDC cutover or OIDC reboot validation was added by this package. DNS and operational/security findings stay open. See [Package Reconciliation](Package-Reconciliation.md) for source preservation, archive hash and exact file mapping.
