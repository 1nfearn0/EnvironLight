# Project map

| Path | Purpose |
|---|---|
| `apps/environlight_s3` | Complete ESP32-S3 N16R8 EnvironLight application and partition/config defaults |
| `apps/radioscout_esp32` | Future DOIT ESP32-WROOM-32D radio-only target; metadata option disabled |
| `components/ec_core` | Shared snapshots, events, scientific maths, binary protocol and bounded history |
| `components/ec_sensors` | Native I2S, AS7341, BMP280 and DHT22 acquisition workers |
| `components/ec_radio` | Wi-Fi connection/survey, passive BLE observation and persistent named-device registry |
| `components/ec_web` | HTTP, WebSocket, controls, exports and embedded dashboard assets |
| `web` | Eight-view responsive dashboard and local plotting library |
| `tests` | Portable C and JavaScript contract/regression tests |
| `tools` | Build, test and deterministic asset-embedding helpers |
| `docs/specs` | Canonical v0.5 product and engineering contract |

Start with `README.md`. Each application is a normal ESP-IDF project and uses
the shared `components` directory through `EXTRA_COMPONENT_DIRS`.
