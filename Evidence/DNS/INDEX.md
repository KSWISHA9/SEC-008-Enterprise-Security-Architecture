# DNS Evidence Index

DNS-001 remains open. Filenames are retained as historical labels; words such as "restored" or "removed" in a filename do not establish overall DNS resolution.

| Evidence | Control / Check | Expected Result | Actual Result and Limit |
| --- | --- | --- | --- |
| [DNS-09](DNS-09-NAT-Record-Still-Returned.png) | DC resolution, discovery and DNS health | Intended DC address and valid delegation | DC hostname returns both 10.10.10.10 and 10.0.2.15; discovery succeeds; delegation test fails in this earlier capture |
| [DNS-14](DNS-14-Pre-Remediation-Snapshot.png) | Configuration export before change | DNS/registry exports succeed | Export commands and success messages visible; backup contents and restoration not verified |
| [DNS-21 short queries](DNS-21-Kali-Client-Validation.png) | DC, domain-root and GC A queries | Results conform to documented intended records | Two addresses visible for three queries; detailed capture below identifies which query lacks an answer |
| [DNS-21 detailed queries](DNS-21-Kali-Client-Residual-Finding.png) | Direct queries to 10.10.10.10 | Intended root answer and working DC/GC records | DC and GC resolve to 10.10.10.10; corp.omniverse.com returns NOERROR with ANSWER: 0 and AUTHORITY: 1 |

The latest root result is a no-data response in the displayed A query, not a displayed NXDOMAIN error or proof of ongoing NAT contamination. The intended domain-root record policy and comparison across clients remain unverified. Keep [DNS-001](../../Findings/REGISTER.md) open until the intended behavior and AD locator health are established.

Other DNS-01 through DNS-20 captures are preserved as supporting chronology. The different-hash DNS-15 filename collision remains documented in [Duplicate-Evidence-Review.md](../../Reports/Duplicate-Evidence-Review.md); no existing image was overwritten.

## Remaining Historical Captures

Expected result for these checks is agreement with the intended authoritative records, intact AD discovery and stable registration behavior. Each frame below is a point-in-time observation, not closure of DNS-001. Existing DNS-09, DNS-14 and both DNS-21 frames are explained above.

| Screenshot | Check and visible result |
| --- | --- |
| [DNS-01](DNS-01-Initial-Failure.png) | Initial DNS health: broken _msdcs delegation and failed DNS test; DC lookup includes lab and NAT addresses |
| [DNS-02](DNS-02-Initial-Records.png) | Record/zone inventory: DC A records include 10.10.10.10 and 10.0.2.15; AD-integrated zones and delegation object displayed |
| [DNS-03](DNS-03-Stale-Delegation.png) | Delegation points at earlier hostname; Internet and Enterprise LAN registration both enabled |
| [DNS-04](DNS-04-NAT-Registration-Disabled.png) | Internet registration changed to false, Enterprise LAN remains true; DC A response still has NAT address |
| [DNS-05](DNS-05-MSDCS-Before.png) | _msdcs zone NS points to OV-DC01 but parent delegation points to earlier hostname; LDAP SRV and old record reference visible |
| [DNS-06](DNS-06-MSDCS-Change.png) | Parent delegation replacement confirmed; updated _msdcs NS points to OV-DC01 |
| [DNS-07](DNS-07-AD-Discovery-Passed.png) | nltest succeeds; LDAP SRV targets OV-DC01 with A/AAAA additional records; does not establish root A behavior |
| [DNS-08](DNS-08-Delegation-Validation.png) | Updated delegation and DNS diagnostic success message; not a complete domain-root validation capture |
| [DNS-10](DNS-10-Post-Change-Residual-Finding.png) | Corrected delegation and DCDIAG exit 0 coexist with DC NAT A record; interface registration states false/true |
| [DNS-11](DNS-11-Netlogon-Registration-Investigation.png) | Adapter inventory and Netlogon-generated record file include NAT-address root/GC/partition records despite interface registration disabled |
| [DNS-12](DNS-12-Domain-and-GC-Address-Contamination.png) | Domain/DC address listings contain unwanted NAT/network addresses; GC list includes NAT; registry fields not populated in frame |
| [DNS-13](DNS-13-Netlogon-and-Stale-Records.png) | Generated Netlogon A records and stored root records show multiple addresses; no applied override value displayed |
| [DNS-15](DNS-15-Netlogon-A-Registration-Control.png) | Registry export success and RegisterDnsARecords set to 0; not proof of enduring registration correction |
| [DNS-16](DNS-16-AD-Locator-Records-Restored.png) | Commands report addition of partition/GC A records at 10.10.10.10; root/DC listings still include unwanted addresses |
| [DNS-17](DNS-17-Unwanted-Addresses-Removed.png) | Cleanup reports root NAT/network record removal; root/DC list 10.10.10.10 and DCDIAG exit 0 at this point |
| [DNS-18](DNS-18-Post-Netlogon-Restart-Validation.png) | Netlogon restart, running services, DCDIAG exit 0 and successful DC discovery; long-term stability not established |
| [DNS-19](DNS-19-Kali-Client-DNS-Validation.png) | Kali DC query still returns lab/NAT addresses, root short answer blank, GC/LDAP SRV present; ping 4/4 |
| [DNS-20](DNS-20-Controlled-DNS-Cleanup.png) | Controlled root NAT cleanup, inventory export message, locator records and DCDIAG 0; explicitly says long-term registration source unconfirmed |
