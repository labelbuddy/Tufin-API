# Tufin API Reference Library

Reference library for the **Tufin SecureTrack REST API (R25-2)** and **GraphQL API**.

## How This Repo Works

- **Source data:** Raw Swagger JSON files live in `Tufin API Downloads/SecureTrack/` and `Tufin API Downloads/SecureChange/`
- **Scripts:** Ready-to-run GraphQL queries/mutations in [`scripts/`](scripts/)
- **Reference docs:** API endpoint docs created on-demand in `graphql/` and REST category folders
- **Workflow:** Describe a problem → relevant docs and scripts are created → solution is built

---

## Scripts

| Script | Description |
|--------|-------------|
| [scripts/get-devices.md](scripts/get-devices.md) | Find device IDs by name, vendor, or pattern |
| [scripts/stale-rule-recertification.md](scripts/stale-rule-recertification.md) | Recertify, disable, or delete stale rules via SecureChange tickets |

---

## GraphQL API Reference

GraphQL endpoint: `https://{tufin-server}/v2/api/sync/graphiql`

| Doc | Description |
|-----|-------------|
| [graphql/rules/query-rules-by-last-hit.md](graphql/rules/query-rules-by-last-hit.md) | Query rules filtered by last-hit date using TQL |
| [graphql/ruleOperations/create-ticket-draft.md](graphql/ruleOperations/create-ticket-draft.md) | Create SecureChange ticket drafts for rule batches |

---

## REST API Categories (SecureTrack R25-2)

Source JSON for all categories is in `Tufin API Downloads/SecureTrack/`. Reference docs are
created in category folders as needed.

| Category | Endpoints |
|----------|-----------|
| Additional Policy Fields | 4 |
| Application IDs | 4 |
| Change Authorization | 2 |
| Change Windows | 4 |
| Device Interfaces and Zones | 5 |
| Domains | 4 |
| General Properties | 2 |
| IPsec VPN | 4 |
| Internet Objects | 5 |
| LDAP | 4 |
| Licenses | 3 |
| Monitored Devices | 19 |
| NAT Policies | 4 |
| Network Objects | 7 |
| Network Topology | 81 |
| Network Zone Manager - Patterns | 3 |
| Network Zone Manager - Subnets | 8 |
| Network Zone Manager - Zones | 20 |
| Policies and Sub-Policies | 11 |
| Policy Analysis | 1 |
| Policy Browser (formerly Rule Documentation) | 7 |
| Policy Optimization | 8 |
| Revisions | 3 |
| Rule Usage | 2 |
| Secure Cloud Integration | 8 |
| Security Rules | 8 |
| Services and Ports | 7 |
| Time Objects | 3 |
| Traffic Policy Matcher | 1 |
| USP - Access Request Violations | 5 |
| USP - Alerts | 5 |
| USP - Cloud Tag Policy | 7 |
| USP - Exceptions | 8 |
| USP - Security Zone Matrix | 6 |
| USP - Violations | 2 |

**Total: 275 REST endpoints across 35 categories**
