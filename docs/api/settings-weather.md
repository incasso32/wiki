---
sidebar_position: 9
title: Weather Settings
---

# Weather Settings

Location and units for the **Weather page**.

---

## `GET /api/setting/weather`

### Response

```json
{
  "WeatherCity":      "Spanbroek, Noord-H, Nederland",
  "WeatherLat":       "52.69843",
  "WeatherLon":       "4.959841",
  "WeatherTempUnit":  "celsius",
  "WeatherSpeedUnit": "kmh",
  "WeatherAltMode":   "pressure"
}
```

| Field              | Type   | Meaning                                                          |
| ------------------ | ------ | ---------------------------------------------------------------- |
| `WeatherCity`      | string | Human-readable city label (used on screen).                       |
| `WeatherLat`       | string | Latitude, decimal degrees.                                       |
| `WeatherLon`       | string | Longitude, decimal degrees.                                      |
| `WeatherTempUnit`  | string | `"celsius"` or `"fahrenheit"`.                                   |
| `WeatherSpeedUnit` | string | `"kmh"`, `"mph"` or `"ms"`.                                      |
| `WeatherAltMode`   | string | `"pressure"`, `"humidity"`, etc.                                 |

---

## `POST /api/setting/weather`

Partial update. Triggers a one-shot refresh.

### Request

```json
{
  "WeatherCity": "Berlin, Berlin, Germany",
  "WeatherLat":  "52.5200",
  "WeatherLon":  "13.4050"
}
```

### Response

`200 OK` with body `OK`.

---

## `POST /api/weather/refresh`

Forces an immediate weather + AQI pull.

### Request

No body.

### Response

```
200 OK   — refresh triggered
429      — rate limited; you sent a refresh in the last 60 s
```

`429` body looks like:

```
Rate limited, please wait 42s before refreshing again.
```

### Example

```bash
curl -X POST http://192.168.1.42/api/weather/refresh
```

:::tip
This endpoint is meant for **manual** "I just changed the city, refresh now". For periodic polling, just let the device do its own background refresh — calling this from a cron job will get you `429`s.
:::
