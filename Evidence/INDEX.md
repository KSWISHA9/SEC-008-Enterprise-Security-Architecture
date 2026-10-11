# Current Evidence Index

Historical evidence records earlier stages. TLS/IAM captures extend the project; they do not convert earlier tests into current-stack retests.

| Category | Index | Scope |
| --- | --- | --- |
| DNS | [DNS index](DNS/INDEX.md) | Investigation chronology; domain-root discrepancy open |
| Segmentation | [Segmentation index](Segmentation/INDEX.md) | Earlier UFW baseline, allow/deny, logs and service/persistence limits |
| Authentication | [Authentication index](Authentication/INDEX.md) | Earlier Basic/HTTP tests, not OIDC |
| Recovery | [Recovery index](Recovery/INDEX.md) | Earlier 8080 stop/start and remote negative checks |
| TLS | [TLS index](TLS/INDEX.md) | CA/certificate, managed 8443 HTTPS and retirement of 8080 |
| IAM | [IAM index](IAM/INDEX.md) | Keycloak, client/roles, test-port rule and RBAC UI outcomes |
| Monitoring | [Monitoring index](Monitoring/INDEX.md) | Existing local firewall events; not complete central identity monitoring |
| Archive | [Archive index](Archive/INDEX.md) | Failed/superseded attempts and recovery preparation |
| AD baseline | [Baseline index](BASELINE-INDEX.md) | Original DC adapter, domain and discovery captures, with historical filename caveats |

The [22-capture primary set](PRIMARY-INDEX.md) is the earlier segmentation/authentication/recovery selection, not the final total. The current inventory contains **73 PNGs**, including **19 unique newly copied TLS/IAM images**. Four duplicate pairs in the new source set were matched by SHA-256; originals remain outside the project.

- [New source/copy provenance and duplicate mappings](TLS-IAM-Provenance.csv)
- [Current 73-image SHA-256 inventory](evidence-sha256-TLS-IAM-current.csv)
- [Current publication decisions](Publication-Review-TLS-IAM.csv)
- [Test outcomes and evidence limits](../Reports/Test-Validation-Report.md)
- [Publication review](../Reports/Publication-Review.md)

Prior inventories and naming maps are historical snapshots, retained unchanged. Screenshots are primary output evidence; operator statements fill only explicitly labeled context. Neither a filename nor a UI page establishes every token, endpoint or runtime property.

## Supplied Package Manifest

The [supplied IAM/TLS manifest](Packages/IAM-TLS-Provided-Manifest.csv) has 19 rows and 17 unique hashes, all already present. The [reconciliation](Packages/Manifest-Reconciliation-20261010.csv) maps its names to existing accurate paths. The second Authentication/Recovery/TLS ZIP is also received and verified: [its evidence index](Packages/Additional-Authentication-Recovery-TLS/INDEX.md) maps 12 included images plus two prior TLS references to existing files. No duplicate image was added; total remains 73. [Package review and label corrections](../Reports/Package-Reconciliation.md) are recorded separately from test outcomes.
