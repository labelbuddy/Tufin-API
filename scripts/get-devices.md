# Get Devices

Run these queries in your Tufin GraphiQL interface:
`https://{tufin-server}/v2/api/sync/graphiql`

Use this to look up a device's numeric `id` — required before running the
[stale-rule-recertification](stale-rule-recertification.md) script.

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

The `id` value in the response is the numeric device ID used in the rules query filter:

```graphql
# Use the id value from above in the rules query
{
  rules(filter: "device.id='42' and timeLastHit before 182 days ago") {
    count
  }
}
```
