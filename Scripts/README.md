# Scripts

No original SEC-008 firewall, network, systemd or DNS automation script was found in the accessible project folder or the scoped text search across Downloads, Desktop, Documents and C:/Terraform. Commands visible in screenshots remain evidence; they have not been reconstructed into scripts or presented as executed automation.

The authentication screenshot references `/home/analyst/secx001-app/auth_server.py` and `auth.conf` on the Ubuntu VM. Those files were not accessed, exported or imported during this organization task. Valid credentials are entered through hidden prompts in the successful Python test captures. Do not add authentication configuration or secrets to this folder.

`Downloads/SECX-001-Segmentation-Evidence-Organized.zip` contains an `Install-Evidence.ps1` organization helper. It was inspected as text but not executed or imported: its package source layout differs from the curated project, and the packaged images already match project evidence by SHA-256. The archive remains intact.

Future lab scripts should be reviewed for secrets and matched to their test evidence before publication. Stored UFW rules, network configuration and the application systemd unit are still evidence gaps.

## TLS and OIDC Extension

TLS/IAM screenshots now show configuration commands and Keycloak service output, but the current application source, secret/session configuration, certificate/private-key files and full units were not accessed or imported. Commands in screenshots are not reconstructed as executed automation. The operator confirms a manually launched OIDC test app on 8445; sanitized source/configuration review remains separate follow-up. Do not export client secrets, session secrets, raw tokens, authorization codes or private keys into this folder.
