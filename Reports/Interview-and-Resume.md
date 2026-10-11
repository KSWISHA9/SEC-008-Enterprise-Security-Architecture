# Interview STAR Explanation and Resume Bullets

Use the project as a **simulated lab engagement**, not a real production tenant/client implementation. Named accounts and port context come from the operator's tests where cropped screenshots omit them. Do not claim full token security review, completed cutover or production readiness.

## STAR Explanation

**Situation:** I built a VirtualBox security lab with Windows Server AD/DNS, Ubuntu and Kali to evaluate protection of an internal application that initially had unnecessary network exposure and plaintext HTTP access.

**Task:** Reduce exposure, protect transport, distinguish authentication from authorization, and validate both successful and denied requests while maintaining usable evidence and recording remaining risks.

**Action:** I applied UFW default-deny inbound and source/port-specific application rules, tested approved and unauthorized clients, and investigated matching firewall denial events. I moved the managed application to lab-CA HTTPS and retired 8080. I configured Keycloak 26.8.0 with realm roles and a confidential OIDC authorization-code client, then tested a separate application on 8445. I used analyst and viewer accounts to validate the analyst-only route and kept recovery tests scoped to the application version actually tested.

**Result:** CA-verified HTTPS remained reachable while the old HTTP path was unavailable. The analyst dashboard was authorized, while the authenticated viewer was denied. I documented open DNS and operational risks instead of claiming a completed enterprise rollout: 8443 still runs Basic, the OIDC test app is manual, and cutover/persistence/key/session hardening remain unfinished.

## Short Interview Version

I built a layered security lab that combines host-based microsegmentation, verified HTTPS and Keycloak OIDC/RBAC. The important test was not just successful login: an authenticated viewer was denied the analyst dashboard. I can explain the network, transport and application boundaries, show the evidence, and describe exactly what remains before promotion of the manual test app.

## Resume-Ready Bullets

Use two or three under a project entry titled **OmniVerse Security Architecture Lab | UFW, TLS, Keycloak, OIDC, RBAC**:

- Engineered host-based microsegmentation in a VirtualBox lab using UFW default-deny inbound and source/port-specific rules; validated approved access, unauthorized-client denial and correlated firewall events.
- Deployed private-lab-CA HTTPS for an Ubuntu internal application, verified certificate trust from Kali, and retired the earlier HTTP listener/access rule.
- Integrated a separate test application with Keycloak 26.8.0 using a confidential OIDC authorization-code client; demonstrated analyst access and authenticated-viewer denial for an analyst-only dashboard.
- Produced architecture/data flows, an evidence-backed control matrix and a remediation register covering unfinished cutover, persistence, CA key separation, DNS and session/token hardening.

Do not add fabricated scale, savings, production uptime, enterprise federation, completed 8443 OIDC deployment, disaster recovery or compliance certification. The earlier stop/start exercise can be discussed, but should not be described as recovery of the current OIDC stack.
