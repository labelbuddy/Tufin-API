# Stale Rule Recertification — GraphQL Script

Run these queries and mutations in your Tufin GraphiQL interface:
`https://{tufin-server}/v2/api/sync/graphiql`

> **Note:** The labels after `query` and `mutation` (e.g. `query { ... }`) are just
> for readability — they are not API calls. Only the field names inside the braces
> (`rules`, `ruleOperations`, etc.) are the actual API calls.

---

## How to Use This Script

1. **Count** — Run the count query to see how many rules match
2. **Retrieve UIDs** — Run the rules query to collect UIDs (paginate if needed)
3. **Submit** — Run the mutation once per batch of 300 UIDs

Replace `YOUR_DEVICE_ID` with the numeric device ID from SecureTrack before running.
Use the [get-devices](get-devices.md) script to look up your device ID.

---

## Step 1 — Recertify (Rules not hit in 182+ days)

### 1a. Count matching rules

```graphql
{
  rules(filter: "deviceId='YOUR_DEVICE_ID' and timeLastHit before 182 days ago") {
    count
  }
}
```

> **Include never-hit rules:** Change the filter to:
> `"deviceId='YOUR_DEVICE_ID' and (timeLastHit before 182 days ago or not timeLastHit exists)"`

---

### 1b. Retrieve rule UIDs

**Page 1 (rules 1–500):**
```graphql
{
  rules(
    filter: "deviceId='YOUR_DEVICE_ID' and timeLastHit before 182 days ago"
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

**Page 2 (rules 501–1000) — increment offset by 500 each time:**
```graphql
{
  rules(
    filter: "deviceId='YOUR_DEVICE_ID' and timeLastHit before 182 days ago"
    first: 500
    offset: 500
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

> Copy the `uid` values from the response. These go into the mutation below.

---

### 1c. Create recertification ticket draft (run once per batch of 300 UIDs)

```graphql
mutation {
  ruleOperations {
    createTicketDraft(
      workflowType: RECERTIFY_RULES
      subject: "Rule Recertification - [Device Name] - Batch 1"
      ruleUids: [
        "PASTE_UID_1_HERE",
        "PASTE_UID_2_HERE",
        "PASTE_UID_3_HERE"
      ]
    ) {
      resultStatus {
        status
        errorMessage
      }
      validRuleUids
      invalidRules {
        ruleUid
        reason
      }
    }
  }
}
```

> **Dry run:** Add `dryRun: true` to validate UIDs without creating a real ticket:
> ```graphql
> createTicketDraft(
>   workflowType: RECERTIFY_RULES
>   dryRun: true
>   ruleUids: ["uid1", "uid2"]
> )
> ```

---

## Step 2 — Disable (Rules not hit in 365+ days)

### 2a. Count matching rules

```graphql
{
  rules(filter: "deviceId='YOUR_DEVICE_ID' and timeLastHit before 365 days ago") {
    count
  }
}
```

---

### 2b. Retrieve rule UIDs

```graphql
{
  rules(
    filter: "deviceId='YOUR_DEVICE_ID' and timeLastHit before 365 days ago"
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

---

### 2c. Create disable ticket draft (run once per batch of 300 UIDs)

```graphql
mutation {
  ruleOperations {
    createTicketDraft(
      workflowType: DECOMMISSION_RULES
      decommissionRulesAction: DISABLE_RULES
      subject: "Rule Disable - [Device Name] - Batch 1"
      ruleUids: [
        "PASTE_UID_1_HERE",
        "PASTE_UID_2_HERE",
        "PASTE_UID_3_HERE"
      ]
    ) {
      resultStatus {
        status
        errorMessage
      }
      validRuleUids
      invalidRules {
        ruleUid
        reason
      }
    }
  }
}
```

---

## Step 3 — Delete (Rules not hit in 730+ days)

### 3a. Count matching rules

```graphql
{
  rules(filter: "deviceId='YOUR_DEVICE_ID' and timeLastHit before 730 days ago") {
    count
  }
}
```

---

### 3b. Retrieve rule UIDs

```graphql
{
  rules(
    filter: "deviceId='YOUR_DEVICE_ID' and timeLastHit before 730 days ago"
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

---

### 3c. Create delete ticket draft (run once per batch of 300 UIDs)

```graphql
mutation {
  ruleOperations {
    createTicketDraft(
      workflowType: DECOMMISSION_RULES
      decommissionRulesAction: REMOVE_RULES
      subject: "Rule Delete - [Device Name] - Batch 1"
      ruleUids: [
        "PASTE_UID_1_HERE",
        "PASTE_UID_2_HERE",
        "PASTE_UID_3_HERE"
      ]
    ) {
      resultStatus {
        status
        errorMessage
      }
      validRuleUids
      invalidRules {
        ruleUid
        reason
      }
    }
  }
}
```

---

## Batching Guide

When you have more than 300 matching rules, run the mutation multiple times:

| Batch | UIDs to include | Change subject to |
|-------|----------------|-------------------|
| Batch 1 | UIDs 1–300 | `"... - Batch 1"` |
| Batch 2 | UIDs 301–600 | `"... - Batch 2"` |
| Batch 3 | UIDs 601–900 | `"... - Batch 3"` |

The `invalidRules` field in each response shows any UIDs that were rejected and why.

---

## Reference

| Topic | Doc |
|-------|-----|
| `rules` query fields and TQL filtering | [graphql/rules/query-rules-by-last-hit.md](../graphql/rules/query-rules-by-last-hit.md) |
| `createTicketDraft` full spec | [graphql/ruleOperations/create-ticket-draft.md](../graphql/ruleOperations/create-ticket-draft.md) |
