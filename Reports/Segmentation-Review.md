# Segmentation Evidence Review - 2026-10-10

## Scope and Repository State

Project: `SECX-001 - Enterprise Security Architecture & Zero Trust`.

Historical segmentation review. The later authentication/recovery captures, including newly found journal boot evidence, are assessed in [Final-Technical-Report.md](Final-Technical-Report.md); this phase record is retained for chronology.

The local SEC-008 folder has no Git metadata; `git status` and `git remote -v` both report that it is not a Git repository. An authenticated GitHub CLI listing of up to 200 repositories owned by KSWISHA9 returned no SECX/SecurityX-named repository. No repository was initialized, committed, or pushed.

Original screenshots are in `Pictures/Screenshots`. All existing project images and original screenshots were retained. The twelve primary SEG images were already present and were visually rechecked without replacement. Two distinct supporting captures were copied into `Evidence/Segmentation/Supplemental` using new filenames and no-overwrite copy operations.

## Supported Controls and Results

| Control | Observed result | Evidence and limits |
| --- | --- | --- |
| Baseline exposure | SSH and application available before restrictive rules | SEG-01 has SSH open and 80/443 closed; SEG-02 has 22/8080 open and HTTP 200 |
| Initial firewall enforcement | Kali sees 22/8080 filtered; HTTP times out | SEG-03; this is an earlier stage before application approval |
| Administrative access | Windows SSH session reaches Ubuntu shell | SEG-04; alias/forwarding configuration not visible |
| Application microsegmentation | Kali reaches 8080; Kali SSH filtered | SEG-05; SEG-06 shows configured allowed source 10.10.10.100 |
| Active UFW policy | Default deny inbound; allow outbound; routed disabled | SEG-12; SSH allows Anywhere on NAT-facing enp0s3, including IPv6 |
| Unauthorized-client denial | Windows TCP test fails, ping succeeds | SEG-07; source 10.10.10.1 to target 10.10.10.101:8080 |
| Denial logging | Matching source/target/port logged as UFW BLOCK | SEG-11 and SEG-12 |
| Managed application | Service active/enabled; HTTP 200 | SEG-09; immediate listener query has no data row |
| Reported persistence test | Expected IPs, active firewall, active/enabled service, listener and HTTP 200 | SEG-10; reboot marker and remote post-reboot tests absent |

## Duplicate and Alternate Capture Handling

Full hashes and original names are available in [source-provenance.csv](../Evidence/Segmentation/source-provenance.csv). Each of the fourteen organized segmentation images has at least one original with exactly matching bytes. Twenty source-to-destination matches were recorded. Existing inventory entries were rehashed and all matched. The new [reviewed inventory](../Evidence/evidence-sha256-reviewed-20261010.csv) includes the supplemental images while retaining the earlier inventory unchanged.

Two similarly named pairs have different hashes and were not treated as exact duplicates:

- SEG-02 `.png.png` and `.pngg.png` show substantially the same baseline HTTP/scan content. The existing primary image was retained; both originals remain in place.
- SEG-05 `.png` is a scan-only result. SEG-05 `.pngg.png` includes both scan and HTTP output and matches the existing primary SEG-05. The scan-only capture was retained as a supplemental image with an accurate description.

## Generic Screenshot Dispositions

| Original screenshot suffix | Review result | Disposition |
| --- | --- | --- |
| 142927 | VirtualBox machine inventory | Preserved outside segmentation; not a control test |
| 144921 | Boot/driver console | Preserved; cannot link to the reported segmentation reboot |
| 150401 | DNS/Netlogon records | Preserved; separate DNS investigation |
| 150846 | Tiny cropped image with no useful test content | Preserved; excluded from evidence claims |
| 150919 | DNS backup commands and successful export messages | Preserved; separate DNS investigation |
| 154651 | Maintenance/password-reset console and command errors | Preserved; excluded from segmentation |
| 155127 | SSH installation, service status, incomplete interface setup | Preserved; setup context, not final segmentation state |
| 161025 | VirtualBox clipboard process | Preserved; unrelated to firewall testing |
| 161221 | Exact match to primary SEG-05 | No extra copy needed |
| 161549 | Exact match to primary SEG-06 | No extra copy needed |
| 161631 | Additional successful Kali HTTP response | Copied to Supplemental |
| 161943 | Exact match to primary SEG-07 | No extra copy needed |
| 162530 | Exact match to primary SEG-08 | No extra copy needed |
| 162659 | Exact match to primary SEG-09 | No extra copy needed |
| 162841 | Exact match to primary SEG-10 | No extra copy needed |
| 163115 | Original for primary SEG-11 | Primary retained |
| 163125 | Separate failed Windows connection-test capture | Preserved; primary SEG-07 already covers this result |
| 163151 | Original for primary SEG-12 | Primary retained |

## Remaining Findings and Evidence Gaps

- DNS-001 remains **Open** in [Open-Findings.md](Open-Findings.md). No DNS remediation or new resolution claim was made.
- Reboot persistence needs paired boot IDs/uptime or reboot logs and remote approved/unauthorized retests.
- Stored network configuration, systemd unit and sanitized UFW export are still missing.
- The NAT-facing SSH rule allows any source on that interface. Source restriction and exposure review remain design follow-up items; this is not evidence of source-IP-limited administration.
- IPv6 access behavior and outbound/routed controls have not been tested by these captures.
- Architecture, client inventory and change-record artifacts remain listed in [Missing-Evidence.md](Missing-Evidence.md).

## Proposed Changes for Review

- Update README, segmentation index and missing-evidence report to reflect exactly what is visible.
- Add this review report, source provenance and a refreshed inventory.
- Add two supplemental PNGs; preserve all twelve existing primary PNGs and all originals.
- Keep the DNS open finding and separate SEC-001/SEC-002 projects unchanged.

Original document versions and a generated `git diff --no-index` review are preserved in the current Codex workspace, outside the project. This is a file comparison against the before-state, not a repository commit diff. No VM, network, firewall or service commands were executed.
