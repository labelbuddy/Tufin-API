# Get Devices

Run these queries in your Tufin GraphiQL interface:
`https://{tufin-server}/v2/api/sync/graphiql`

Use this to look up a device's numeric `deviceId` — required before running the
[stale-rule-recertification](stale-rule-recertification.md) script.

---

## Get all devices

```graphql
query GetAllDevices {
  devices {
    count
    values {
      deviceId
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
query FindDeviceByName {
  devices(filter: "name='YOUR_DEVICE_NAME'") {
    count
    values {
      deviceId
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
query FindDevicesByVendor {
  devices(filter: "vendor='Check Point'") {
    count
    values {
      deviceId
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
query FindDevicesByNamePattern {
  devices(filter: "name contains 'FW'") {
    count
    values {
      deviceId
      name
      vendor
      model
    }
  }
}
```

---

## Response

The `deviceId` in the response is the numeric ID used as input to other queries and mutations,
for example:

```graphql
# Use deviceId in the rules query
rules(filter: "deviceId='42' and timeLastHit before 182 days ago")
```
