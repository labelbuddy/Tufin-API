# Get Devices

**GET** `/securetrack/api/devices/`

## Description
Returns the list of devices that are configured in SecureTrack, including the vendor, model, name, domain and device ID.
The results can be sorted by ip, name, vendor and model.

## Parameters
| Name | In | Required | Type | Description |
|------|----|----------|------|-------------|
| context | query | No | integer | Global MSSP context |
| name | query | No | string | Device name |
| ip | query | No | string | Device IP address |
| vendor | query | No | string | Device vendor |
| model | query | No | string | Device model |
| sort | query | No | string | Sort ascending or descending — allowable values: `asc`, `desc` |
| start | query | No | string | Starting page for query (numeric) |
| count | query | No | string | Number of pages for query starting from starting page (numeric) |
| show_os_version | query | No | boolean | Include OS version in the response |
| show_license | query | No | boolean | Include license information for each device |
| status | query | No | string | Device status — allowable values: `started`, `stopped`, `connected`, `disabled`, `error` |

## Example Requests
```
GET https://{tufin-server}/securetrack/api/devices
GET https://{tufin-server}/securetrack/api/devices?vendor=VMware
GET https://{tufin-server}/securetrack/api/devices?sort=ip:asc
GET https://{tufin-server}/securetrack/api/devices?show_os_version=true
GET https://{tufin-server}/securetrack/api/devices?status=started
```

## Response Codes
| Code | Message |
|------|---------|
| 200 | Successful |

## Content Types
`application/xml`, `application/json`
