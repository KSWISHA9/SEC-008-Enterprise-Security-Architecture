# SECX-001 Additional Evidence

This is a supplemental archive to SECX-001-Reviewed-IAM-TLS-Evidence.zip.

Evidence folders: Authentication, Recovery, TLS, Supplemental. All screenshots are preserved without image edits. Byte-identical duplicate screenshots were excluded; the manifest identifies screenshots already in the prior ZIP.

Chronology matters: Basic Authentication and Recovery screenshots were taken while the application ran on HTTP port 8080. TLS screenshots document later HTTPS on 8443. Keycloak/OIDC role enforcement was validated on the isolated test application on 8445, NOT cut over to production 8443.

Caveats: Historical HTTP screenshot is not evidence of current open port 8080. Curl returning HTTP 401 with purported valid credentials is an unresolved client-specific test discrepancy; Python urllib authenticated successfully. No secrets should be committed. DNS root resolution, permanent Keycloak admin, OIDC production cutover, and reboot persistence remain follow-ups.
