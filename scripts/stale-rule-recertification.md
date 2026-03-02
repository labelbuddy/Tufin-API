# Stale Rule Recertification — GraphQL Script

Run these queries and mutations in your Tufin GraphiQL interface:
`https://{tufin-server}/v2/api/sync/graphiql`

---

## How to Use This Script

**Workflow overview for each step:**

1. **Count** — Run the count query to see how many rules match before doing anything
2. **Retrieve UIDs** — Run the rules query to collect rule UIDs (paginate as needed for large firewalls)
3. **Submit** — Run the mutation once per batch of 300 UIDs to create ticket drafts in SecureChange

**Tip:** Use `dryRun: true` in the mutation first to validate your UIDs without creating a real ticket.

Replace `YOUR_DEVICE_ID` with the numeric device ID from SecureTrack before running.

---

## Step 1 — Recertify (Rules not hit in 182+ days)

### 1a. Count matching rules

```graphql
query CountRulesForRecertification {
  rules(filter: "deviceId='YOUR_DEVICE_ID' and timeLastHit before 182 days ago") {
    count
  }
}
```

> **Include never-hit rules:** Change the filter to:
> `"deviceId='YOUR_DEVICE_ID' and (timeLastHit before 182 days ago or not timeLastHit exists)"`

---

### 1b. Retrieve rule UIDs (run multiple times, adjusting `offset` for large firewalls)

**Page 1 (rules 1–500):**
```graphql
query GetStaleRulesForRecertification {
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

**Page 2 (rules 501–1000) — change offset:**
```graphql
query GetStaleRulesForRecertification_Page2 {
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

> Copy the `uid` values from the response. These are used in the mutation below.

---

### 1c. Create recertification ticket draft (run once per batch of 300 UIDs)

```graphql
mutation CreateRecertificationTicket {
  ruleOperations {
    createTicketDraft(
      workflowType: RECERTIFY_RULES
      subject: "Rule Recertification - [Device Name] - Batch 1"
      ruleUids: [
        "PASTE_UID_1_HERE",
        "PASTE_UID_2_HERE",
        "PASTE_UID_3_HERE"
        # ... up to 300 UIDs
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

> **Dry run test:** Add `dryRun: true` to validate UIDs without creating a ticket:
> ```graphql
> createTicketDraft(
>   workflowType: RECERTIFY_RULES
>   dryRun: true
>   ruleUids: ["uid1", "uid2"]
> )
> ```

---

---

## Step 2 — Disable (Rules not hit in 365+ days)

### 2a. Count matching rules

```graphql
query CountRulesForDisable {
  rules(filter: "deviceId='YOUR_DEVICE_ID' and timeLastHit before 365 days ago") {
    count
  }
}
```

---

### 2b. Retrieve rule UIDs

```graphql
query GetStaleRulesForDisable {
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
mutation CreateDisableTicket {
  ruleOperations {
    createTicketDraft(
      workflowType: DECOMMISSION_RULES
      decommissionRulesAction: DISABLE_RULES
      subject: "Rule Disable - [Device Name] - Batch 1"
      ruleUids: [
        "PASTE_UID_1_HERE",
        "PASTE_UID_2_HERE",
        "PASTE_UID_3_HERE"
        # ... up to 300 UIDs
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

---

## Step 3 — Delete (Rules not hit in 730+ days)

### 3a. Count matching rules

```graphql
query CountRulesForDelete {
  rules(filter: "deviceId='YOUR_DEVICE_ID' and timeLastHit before 730 days ago") {
    count
  }
}
```

---

### 3b. Retrieve rule UIDs

```graphql
query GetStaleRulesForDelete {
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
mutation CreateDeleteTicket {
  ruleOperations {
    createTicketDraft(
      workflowType: DECOMMISSION_RULES
      decommissionRulesAction: REMOVE_RULES
      subject: "Rule Delete - [Device Name] - Batch 1"
      ruleUids: [
        "PASTE_UID_1_HERE",
        "PASTE_UID_2_HERE",
        "PASTE_UID_3_HERE"
        # ... up to 300 UIDs
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

When you have more than 300 matching rules, split them across multiple mutation runs:

| Batch | UIDs to include | Mutation subject |
|-------|----------------|-----------------|
| Batch 1 | UIDs 1–300 | `"Rule Recertification - FW01 - Batch 1"` |
| Batch 2 | UIDs 301–600 | `"Rule Recertification - FW01 - Batch 2"` |
| Batch 3 | UIDs 601–900 | `"Rule Recertification - FW01 - Batch 3"` |

The `invalidRules` section in each response will tell you if any UIDs were rejected and why
(e.g., rule no longer exists, already in another ticket, etc.).

---

## Reference

| Topic | Reference doc |
|-------|--------------|
| `rules` query fields and TQL filtering | [graphql/rules/query-rules-by-last-hit.md](../graphql/rules/query-rules-by-last-hit.md) |
| `createTicketDraft` mutation full spec | [graphql/ruleOperations/create-ticket-draft.md](../graphql/ruleOperations/create-ticket-draft.md) |
