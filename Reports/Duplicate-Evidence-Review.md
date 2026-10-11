# Duplicate Evidence Review

## Method

Candidate screenshots were reviewed by filename, visual content, and SHA-256 hash. Original screenshots were not deleted or moved. Verified evidence was copied into the project with standardized names.

## Exact Duplicate Source Screenshots

The following source screenshots had matching SHA-256 hashes and were treated as duplicate captures:

| SHA-256 Prefix | Duplicate Source Names | Kept Evidence Copy |
| --- | --- | --- |
| `082b4475809e` | `Screenshot 2026-10-10 161549.png`; `SEG-06-UFW-Policy-and-Application-Listener.png` | `Evidence/Segmentation/SEG-06-UFW-Policy-and-Application-Listener.png` |
| `54addb7dbbdc` | `Screenshot 2026-10-10 162841.png`; `SEG-10-Reboot-Persistence-Validation.png` | `Evidence/Segmentation/SEG-10-Reboot-Persistence-Validation.png` |
| `6259d06df98` | `Screenshot 2026-10-10 162530.png`; `SEG-08-Persistent-Network-Configuration.png` | `Evidence/Segmentation/SEG-08-Persistent-Network-Configuration.png` |
| `79f7b2cbf16` | `Screenshot 2026-10-10 162659.png`; `SEG-09-Systemd-Managed-Application.png .png` | `Evidence/Segmentation/SEG-09-Systemd-Managed-Application.png` |
| `9732617a174` | `Screenshot 2026-10-10 161221.png`; `SEG-05-Least-Privilege-Application-Access.pngg.png` | `Evidence/Segmentation/SEG-05-Least-Privilege-Application-Access.png` |
| `f01da6c55ec4` | `Screenshot 2026-10-10 161943.png`; `SEG-07-Unauthorized-Client-Blocked.png` | `Evidence/Segmentation/SEG-07-Unauthorized-Client-Blocked.png` |

## Filename Collision Requiring Review

The DNS screenshots included both:

- `DNS-15-Netlogon-A-Registration-Control.png.png`
- `DNS-15-Netlogon-A-Registration-Control.pngg.png`

After filename cleanup, both map to `DNS-15-Netlogon-A-Registration-Control.png`, but they have different SHA-256 hashes. The first file was copied into `Evidence/DNS/`; the second was not overwritten and remains preserved in the original screenshot directory for manual review.

## Notes

- No original evidence was deleted.
- No different existing screenshot was overwritten.
- The project-local evidence inventory is stored at `Evidence/evidence-sha256-inventory.csv`.
