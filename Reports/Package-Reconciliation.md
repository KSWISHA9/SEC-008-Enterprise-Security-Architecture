# Evidence Package Reconciliation

Review date: 2026-10-10. **Both supplied packages reconciled and integrated through existing evidence paths.** No duplicate screenshots were imported.

## Received Material

The supplied Downloads/EVIDENCE-MANIFEST.csv contains 19 rows describing IAM/TLS evidence, with 17 unique SHA-256 hashes. All 19 rows match existing project images exactly. The two repeated hashes are client-settings and HTTP-retirement alternate filenames; the project retains one copy of each image.

- [Unmodified supplied manifest](../Evidence/Packages/IAM-TLS-Provided-Manifest.csv)
- [Hash and path reconciliation](../Evidence/Packages/Manifest-Reconciliation-20261010.csv)

The supplied reviewed paths are metadata, not instructions to overwrite accurate existing filenames or import duplicate files. Its descriptions are reviewed claims, not automatically verified test results. No package instructions or helper script was executed.

## Corrections Verified Against Image Contents

| Source entry | Actual evidence | Project handling |
| --- | --- | --- |
| IAM-04 described as realm admin console | Blank Keycloak sign-in form | Keep IAM-04-Keycloak-Login-Page.png; no authenticated console claim |
| TLS-03 source filename says authenticated HTTPS | Trusted-CA unauthenticated HTTP 401; curl exit 0 | Keep Remote-HTTPS-Authentication-Challenge filename; no authenticated 200 claim |
| IAM-09 description says callback registration not visible | TEST CALLBACK REGISTERED message visible, but URI not shown | Preserve message/URI distinction; final callback validation remains missing |

All existing filenames, screenshot pixels and source files were preserved. Total project image count remains 73. The manifest does not provide new Authentication or Recovery evidence and does not close any prior recovery or OIDC deployment finding.

## Second ZIP - Received and Verified

SECX-001-Additional-Authentication-Recovery-TLS-Evidence.zip was subsequently provided in Downloads. It contains 12 PNGs plus a manifest and README. All 12 PNG hashes match the included manifest and existing project images. The manifest's two additional TLS references are intentionally absent from this ZIP and match existing TLS-03/04 images. All 14 manifest rows therefore resolve to current evidence, without adding duplicate files.

The ZIP was extracted into a separate local review folder with path containment and entry-type checks. All 12 images were visually inspected. No archive helper code or document instruction was executed. Original ZIP, source files and all 73 project images remain unchanged.

- [Second-package evidence index](../Evidence/Packages/Additional-Authentication-Recovery-TLS/INDEX.md)
- [Unmodified package manifest](../Evidence/Packages/Additional-Authentication-Recovery-TLS/Provided-Manifest.csv)
- [Entry/hash/path reconciliation](../Evidence/Packages/Additional-Authentication-Recovery-TLS/Reconciliation.csv)
- [Archive provenance](../Evidence/Packages/Additional-Authentication-Recovery-TLS/Package-Provenance.json)

**Critical naming correction:** the package's AUTH-01 image shows HTTP 000 and a blank listener query, not the manifest's claimed 401. It matches archived TRB-01 and must not overwrite the valid primary AUTH-01. The package's supplemental service-log diagnostic contains the actual 401 and matches primary AUTH-01. The supplemental interactive curl diagnostic matches archived TRB-02: its purported correct-credential attempt returns 401 and is not successful authentication.

AUTH-03/REC-03 are retained as supporting negative-test evidence and the supplemental curl diagnostic as archived troubleshooting under the current owner-approved screenshot decision. No valid password, token, client/session secret or private-key content was observed. Packaging does not change their test results or establish approval to publish.

## Configuration Stages Remain Separate

| Stage | Endpoint and evidence | Limits |
| --- | --- | --- |
| Historical HTTP/Basic | 8080; AUTH and REC images | Recovery covers earlier app; no current-stack recovery inferred |
| Later managed HTTPS/Basic | 8443; private-CA TLS and HTTP retirement | Basic remains; trusted-CA 401 is not authenticated-content success |
| Identity provider and OIDC test | Keycloak 8444; manual test app 8445 | No completed 8443 OIDC cutover or manual-app reboot proof |

DNS stays open. CA key separation, token/session/secret review and current-stack recovery remain outstanding. The blanket rejected-password hold is removed; exact-release review and publication approval remain pending. No staging, commit, push, VM change, screenshot deletion or secret import occurred.
