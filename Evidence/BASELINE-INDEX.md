# Domain Controller Baseline Evidence

Seven original root-level images are retained at their existing paths. Filenames are historical, not independent assertions of controls. Expected baseline checks identify the lab adapters, addresses, domain role and discovery behavior.

| Screenshot | Demonstrates / limitation |
| --- | --- |
| [VirtualBox adapter](SECX001-01-DC-VirtualBox.png) | DC VirtualBox NAT adapter settings; not firewall enforcement |
| [Second adapter](SECX001-02-DC-NAT-Adapter.png) | Host-only second adapter settings despite the historical NAT filename; no renamed overwrite |
| [Network inventory](SECX001-03-DC-Network-Verified.png) | DC network/adapter output with lab and NAT addresses; not full policy validation |
| [Domain identity](SECX001-03-DC-Network.png) | Hostname/domain membership context; distinct capture from verified network inventory |
| [AD inventory](SECX001-04-AD-Inventory-Verified.png) | Adapter/DNS, firewall profile and AD computer inventory; no production-hardening certification |
| [DC discovery](SECX001-05-DC-Domain-Discovery.png) | Successful domain-controller discovery; not DNS-001 closure |
| [Network/firewall baseline](SECX001-06-DC-Network-Firewall-Baseline.png) | Interface/firewall/discovery context; separate from Ubuntu UFW enforcement |

These images support the lab environment, not Keycloak AD federation. No VM configuration was modified during portfolio maintenance.
