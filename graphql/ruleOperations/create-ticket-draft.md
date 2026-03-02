# Create Ticket Draft

**Mutation:** `ruleOperations.createTicketDraft`
**Endpoint:** `https://{tufin-server}/v2/api/sync/graphiql`

## Description
Creates a SecureChange ticket draft for a batch of rules. Supports recertification,
disable, and delete workflows. Maximum 300 rule UIDs per call.

## Mutation Signature

```graphql
mutation {
  ruleOperations {
    createTicketDraft(
      ruleUids: [IdString!]!          # Required — array of rule UIDs (max 300)
      workflowType: WorkFlowType!     # Required — see enum values below
      decommissionRulesAction: DecommissionRulesAction  # Required for DECOMMISSION_RULES
      subject: String                 # Optional — ticket subject line
      workflowName: String            # Optional — target a specific named workflow
      dryRun: Boolean                 # Optional — validate without creating ticket
    ) {
      resultStatus {
        status
        errorMessage
      }
      validRuleUids       # UIDs that were accepted
      invalidRules {
        ruleUid           # UID that was rejected
        reason            # Why it was rejected
      }
    }
  }
}
```

## Parameters

| Name | Required | Type | Description |
|------|----------|------|-------------|
| `ruleUids` | Yes | `[IdString!]!` | Array of rule `uid` values. Max 300 per call. |
| `workflowType` | Yes | `WorkFlowType!` | Workflow type — see enum values below |
| `decommissionRulesAction` | Conditional | enum | Required when `workflowType` is `DECOMMISSION_RULES` |
| `subject` | No | String | Ticket subject shown in SecureChange |
| `workflowName` | No | String | Target a specific SecureChange workflow by name |
| `dryRun` | No | Boolean | `true` = validate UIDs without creating a ticket |

## `workflowType` Enum Values

| Value | Use case |
|-------|----------|
| `RECERTIFY_RULES` | Rule recertification workflow |
| `DECOMMISSION_RULES` | Rule disable or delete (requires `decommissionRulesAction`) |
| `MODIFY_RULES` | Rule modification workflow |

## `decommissionRulesAction` Enum Values

Used only when `workflowType: DECOMMISSION_RULES`.

| Value | Effect |
|-------|--------|
| `DISABLE_RULES` | Rules will be disabled |
| `REMOVE_RULES` | Rules will be deleted |

## Example — Recertify rules

```graphql
mutation {
  ruleOperations {
    createTicketDraft(
      workflowType: RECERTIFY_RULES
      subject: "Rule Recertification - FW01 - Batch 1"
      ruleUids: ["uid-1", "uid-2", "uid-3"]
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

## Example — Disable rules

```graphql
mutation {
  ruleOperations {
    createTicketDraft(
      workflowType: DECOMMISSION_RULES
      decommissionRulesAction: DISABLE_RULES
      subject: "Rule Disable - FW01 - Batch 1"
      ruleUids: ["uid-1", "uid-2", "uid-3"]
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

## Example — Delete rules

```graphql
mutation {
  ruleOperations {
    createTicketDraft(
      workflowType: DECOMMISSION_RULES
      decommissionRulesAction: REMOVE_RULES
      subject: "Rule Delete - FW01 - Batch 1"
      ruleUids: ["uid-1", "uid-2", "uid-3"]
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

## Example — Dry run validation

```graphql
mutation {
  ruleOperations {
    createTicketDraft(
      workflowType: RECERTIFY_RULES
      dryRun: true
      ruleUids: ["uid-1", "uid-2"]
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

## Response

| Field | Description |
|-------|-------------|
| `resultStatus.status` | Overall operation status |
| `resultStatus.errorMessage` | Error detail if failed |
| `validRuleUids` | List of UIDs successfully added to the ticket |
| `invalidRules[].ruleUid` | UID that could not be added |
| `invalidRules[].reason` | Reason the rule was rejected (e.g., already in a ticket, rule not found) |

## Content Types
`application/json`, `application/graphql`
