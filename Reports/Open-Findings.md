# Open Findings

## DNS-001 - Domain-Root DNS Lookup Discrepancy

Status: Open

### Summary

The DNS domain-root issue remains unresolved. Earlier captures document unwanted NAT addresses and delegation errors. The latest detailed Kali query to 10.10.10.10 shows `corp.omniverse.com` returning `NOERROR` with `ANSWER: 0` and `AUTHORITY: 1`, while `OV-DC01.corp.omniverse.com` and `gc._msdcs.corp.omniverse.com` return 10.10.10.10. This is a domain-root A-query discrepancy requiring confirmation of intended record behavior; the latest screenshot does not show continued NAT contamination.

### Evidence

Relevant evidence is stored in:

- `Evidence/DNS/`

Notable screenshots include:

- `DNS-09-NAT-Record-Still-Returned.png`
- `DNS-10-Post-Change-Residual-Finding.png`
- `DNS-21-Kali-Client-Residual-Finding.png`

### Risk

Earlier unintended addresses could direct clients to an incorrect destination. An empty domain-root A answer may affect clients or applications that expect that record, but the intended root-record policy is not yet documented. AD locator health must be checked separately; the displayed A-query results alone do not demonstrate an authentication or domain-discovery outage.

### Current Assessment

This finding should remain open until follow-up validation demonstrates that:

- Domain-root A responses match the explicitly documented intended record policy, including whether a root A record is required.
- AD locator records remain intact.
- Stale NAT or unwanted addresses no longer appear in client validation.
- The remediation remains stable after Netlogon restart and DNS cache refresh.

### Next Steps

- Review the zone root A records and confirm whether any root records are intentionally required.
- Validate Netlogon registration behavior.
- Confirm DNS scavenging and stale-record handling.
- Re-test from the Kali client and a domain-joined Windows client.
- Compare the authoritative zone records and direct Windows/Kali queries after cache refresh.
- Capture final evidence only after the domain-root query returns the documented intended result.

This issue is not marked resolved in SEC-008.

See the [security findings register](../Findings/REGISTER.md) for associated design reviews and validation gaps.
