# Lessons Learned

- **Authentication is not authorization.** The viewer can sign in and retrieve roles while still being denied the analyst route.
- **Networking is only one boundary.** A source allowlist reduces exposure but does not establish device/user trust or protect against a compromised approved client.
- **TLS needs verification.** Trusted-CA requests and matching certificate identity are stronger evidence than a browser page alone; curl exit 0 with 401 proves transport/endpoint availability, not authenticated content.
- **Retirement needs both policy and runtime evidence.** Removing the 8080 rule and checking listener/remote behavior explain the transition more clearly than a renamed file.
- **A test deployment is not a cutover.** 8445 OIDC success does not mean 8443 Basic was replaced or that a manually launched process persists.
- **Operational evidence is version-specific.** Earlier service recovery and boot captures cannot certify a later application/identity stack.
- **CA placement changes the threat model.** Keeping signing trust on the workload host concentrates compromise impact; successful chain validation does not solve key isolation.
- **Role output is not complete token assurance.** Signature/claims, callback protections and session behavior need source review and negative tests.
- **Evidence titles can be wrong.** The original TLS-03 name claimed authenticated access while the image showed 401; descriptions now follow output.
- **A defensible portfolio includes open findings.** DNS, temporary administrator and deployment gaps remain visible rather than being marked resolved for presentation.
- **Publication is a separate review.** Distinguish deliberately rejected test strings from confirmed working credentials. Preserve originals, describe results accurately and review reuse uncertainty and the exact release before publication.
