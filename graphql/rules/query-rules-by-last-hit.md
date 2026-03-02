# Query Rules by Last Hit

**Query:** `rules`
**Endpoint:** `https://{tufin-server}/v2/api/sync/graphiql`

## Description
Returns firewall rules with optional TQL filtering. Used to find stale rules based on
`timeLastHit` date for recertification, disable, or delete workflows.

## Query Signature

```graphql
query {
  rules(
    filter: String    # TQL filter expression
    first: Int        # Max results per page (default 100, max 500)
    offset: Int       # Pagination offset (default 0)
  ) {
    count             # Total matching rules (ignores first/offset)
    values {
      uid             # Rule UID — used as input to ruleOperations mutations
      id              # Internal numeric ID
      name            # Rule name
      deviceId        # Numeric device ID
      timeLastHit     # ISO 8601 datetime of last traffic hit (null if never hit)
      permissivenessLevel  # HIGH / MEDIUM / LOW
    }
  }
}
```

## TQL Filter Reference

| Goal | Filter expression |
|------|-------------------|
| All rules on a device | `deviceId='123'` |
| Not hit in 182+ days | `timeLastHit before 182 days ago` |
| Not hit in 365+ days | `timeLastHit before 365 days ago` |
| Not hit in 730+ days | `timeLastHit before 730 days ago` |
| Never hit | `not timeLastHit exists` |
| Stale or never hit | `timeLastHit before 182 days ago or not timeLastHit exists` |
| Device + stale | `deviceId='123' and timeLastHit before 182 days ago` |

Logical operators: `and`, `or`, `not`

## Parameters

| Name | In | Required | Type | Description |
|------|----|----------|------|-------------|
| filter | query | No | String | TQL expression to filter rules |
| first | query | No | Int | Max rules to return per call (max 500, default 100) |
| offset | query | No | Int | Starting position for pagination |

## Example — Count stale rules

```graphql
query {
  rules(filter: "deviceId='42' and timeLastHit before 182 days ago") {
    count
  }
}
```

## Example — Get rule UIDs with last hit date (paginated)

```graphql
query {
  rules(
    filter: "deviceId='42' and timeLastHit before 182 days ago"
    first: 500
    offset: 0
  ) {
    count
    values {
      uid
      name
      timeLastHit
    }
  }
}
```

## Response Codes

| Condition | Result |
|-----------|--------|
| Success | `count` and `values` populated |
| Invalid TQL | Error in response body |
| No matching rules | `count: 0`, `values: []` |

## Content Types
`application/json`, `application/graphql`
