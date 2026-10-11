# IAM Evidence Index

Fourteen unique screenshots, including supplemental realm-console evidence. No raw tokens, client/session secrets, authorization codes or passwords were visible. Contextual account names and UUIDs remain lab metadata.

| Screenshot | Control / expected result | Actual and limit |
| --- | --- | --- |
| [IAM-01](IAM-01-Keycloak-Realm-and-Roles.png) | Create realm and distinct roles | omniverse created; analyst/viewer listed; realm discovery 200. Earlier administration login uses localhost HTTP; current external TLS is separate evidence |
| [IAM-02](IAM-02-User-Role-Assignments.png) | Separate analyst/viewer role assignments | Created user IDs and role lists differ; discovery 200. This frame does not display usernames or login tests |
| [IAM-03](IAM-03-Confidential-OIDC-Client-Configuration.png) | Confidential standard-flow OIDC client | openid-connect, publicClient=false, standardFlowEnabled=true; initial callback 8443. No secret value displayed |
| [IAM-04](IAM-04-Keycloak-Login-Page.png) | Identity-provider login UI | Blank sign-in form; not proof of authentication or admin-console authorization |
| [IAM-05](IAM-05-OIDC-Settings-and-Temporary-Admin-Warning.png) | Review client authentication and callback | Client authentication on; initial 8443 callback; temporary-admin warning. Does not show final 8445 callback |
| [IAM-06](IAM-06-Keycloak-26-8-0-HTTPS-Service.png) | Identity service available on HTTPS | Keycloak 26.8.0 startup; active/running; wildcard 8444 listener; local 200/exit 0. Prod profile is runtime mode, not readiness attestation |
| [IAM-07](IAM-07-Keycloak-Firewall-Policy.png) | Approved client path for identity endpoint | UFW active; 8443/8444 restricted to 10.10.10.100 on enp0s8; trusted-CA discovery 200 |
| [IAM-08](IAM-08-Remote-Keycloak-and-Application-HTTPS.png) | Kali verifies IdP and managed app reachability | CA-verified discovery 200 on 8444, Basic challenge 401 on 8443; 22/8080 filtered, 8443/8444 open |
| [IAM-09](IAM-09-OIDC-Test-Port-Firewall.png) | Add isolated test path/callback | 8445 approved-source rule visible; TEST CALLBACK REGISTERED message. Actual callback value not displayed; failed sudo prompts have no password value |
| [IAM-10](IAM-10-Required-Profile-Fields.png) | Identify login required-action behavior | Account-information form requires email/first/last names. Not successful login or proof fields were later completed |
| [IAM-11](IAM-11-Analyst-Dashboard-Authorized.png) | Authorized analyst-only resource | Dashboard says Authorized. User ov-analyst and port 8445 operator-confirmed; cropped frame shows neither identity nor HTTP status |
| [IAM-12](IAM-12-Viewer-Identity-and-Realm-Roles.png) | Viewer authenticates without analyst role | ov-viewer signed in; viewer/default realm roles displayed. Role retrieval visible; complete token validation not proved |
| [IAM-13](IAM-13-Analyst-Dashboard-Forbidden.png) | Authenticated viewer denied analyst resource | Forbidden page visible. Association with viewer sequence and numeric HTTP 403 operator-confirmed; frame has no request/status header |
| [Supplemental](IAM-SUP-01-Keycloak-Realm-Console.png) | Realm console context | omniverse welcome and admin context visible; not final hardening proof |

The operator confirms authorization-code login and token validation in the 8445 test app. Initial client settings and observed role/UI outcomes support the integration story; cryptographic/claim validation and session security need source review and negative tests. Do not infer cutover to 8443, manual-app persistence, MFA or AD federation.
