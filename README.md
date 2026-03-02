# Tufin SecureTrack API Reference Library

Reference library for the **Tufin SecureTrack REST API (R25-2)**.

## How This Repo Works

- **Source data:** Raw Swagger JSON files for all 35 API categories live in [`Tufin API Downloads/`](Tufin%20API%20Downloads/)
- **Reference docs:** Markdown files documenting individual endpoints are created on-demand inside category folders
- **Workflow:** Describe a problem → relevant endpoint docs are created → a solution is built

All requests are authenticated via **HTTP Basic Auth** over **HTTPS**.
Base URL: `https://{tufin-server}/securetrack/api`

---

## API Categories

| Category | Endpoints | Folder |
|----------|-----------|--------|
| Additional Policy Fields | 4 | [additional-policy-fields/](additional-policy-fields/) |
| Application IDs | 4 | [application-ids/](application-ids/) |
| Change Authorization | 2 | [change-authorization/](change-authorization/) |
| Change Windows | 4 | [change-windows/](change-windows/) |
| Device Interfaces and Zones | 5 | [device-interfaces-and-zones/](device-interfaces-and-zones/) |
| Domains | 4 | [domains/](domains/) |
| General Properties | 2 | [general-properties/](general-properties/) |
| IPsec VPN | 4 | [ipsec-vpn/](ipsec-vpn/) |
| Internet Objects | 5 | [internet-objects/](internet-objects/) |
| LDAP | 4 | [ldap/](ldap/) |
| Licenses | 3 | [licenses/](licenses/) |
| Monitored Devices | 19 | [monitored-devices/](monitored-devices/) |
| NAT Policies | 4 | [nat-policies/](nat-policies/) |
| Network Objects | 7 | [network-objects/](network-objects/) |
| Network Topology | 81 | [network-topology/](network-topology/) |
| Network Zone Manager - Patterns | 3 | [network-zone-manager-patterns/](network-zone-manager-patterns/) |
| Network Zone Manager - Subnets | 8 | [network-zone-manager-subnets/](network-zone-manager-subnets/) |
| Network Zone Manager - Zones | 20 | [network-zone-manager-zones/](network-zone-manager-zones/) |
| Policies and Sub-Policies | 11 | [policies-and-sub-policies/](policies-and-sub-policies/) |
| Policy Analysis | 1 | [policy-analysis/](policy-analysis/) |
| Policy Browser (formerly Rule Documentation) | 7 | [policy-browser/](policy-browser/) |
| Policy Optimization | 8 | [policy-optimization/](policy-optimization/) |
| Revisions | 3 | [revisions/](revisions/) |
| Rule Usage | 2 | [rule-usage/](rule-usage/) |
| Secure Cloud Integration | 8 | [secure-cloud-integration/](secure-cloud-integration/) |
| Security Rules | 8 | [security-rules/](security-rules/) |
| Services and Ports | 7 | [services-and-ports/](services-and-ports/) |
| Time Objects | 3 | [time-objects/](time-objects/) |
| Traffic Policy Matcher | 1 | [traffic-policy-matcher/](traffic-policy-matcher/) |
| USP - Access Request Violations | 5 | [usp-access-request-violations/](usp-access-request-violations/) |
| USP - Alerts | 5 | [usp-alerts/](usp-alerts/) |
| USP - Cloud Tag Policy | 7 | [usp-cloud-tag-policy/](usp-cloud-tag-policy/) |
| USP - Exceptions | 8 | [usp-exceptions/](usp-exceptions/) |
| USP - Security Zone Matrix | 6 | [usp-security-zone-matrix/](usp-security-zone-matrix/) |
| USP - Violations | 2 | [usp-violations/](usp-violations/) |

**Total: 275 endpoints**
