# Get Devices

Run these queries in your Tufin GraphiQL interface:
`https://{tufin-server}/v2/api/sync/graphiql`

Use this to find the exact device name as it appears in SecureTrack — required before running the
[stale-rule-recertification](stale-rule-recertification.md) script. Rules are filtered using
`device.name='exact name here'`.

---

## Get all devices

```graphql
{
  devices {
    count
    values {
      id
      name
      vendor
      model
    }
  }
}
```

---

## Find a device by name

```graphql
{
  devices(filter: "name='YOUR_DEVICE_NAME'") {
    count
    values {
      id
      name
      vendor
      model
    }
  }
}
```

---

## Find devices by vendor

```graphql
{
  devices(filter: "vendor='Check Point'") {
    count
    values {
      id
      name
      vendor
      model
    }
  }
}
```

---

## Find devices by name pattern (contains)

```graphql
{
  devices(filter: "name contains 'FW'") {
    count
    values {
      id
      name
      vendor
      model
    }
  }
}
```

---

## Response

The `name` value in the response is used directly in rule query filters:

```graphql
# Use the name value from above in the rules query
{
  rules(filter: "device.name='your-device-name' and timeLastHit before 182 days ago") {
    count
  }
}
```
