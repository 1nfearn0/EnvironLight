# EnvironCore Platform v0.5.0

EnvironCore v0.5 is an ESP-IDF 5.5 instrument platform. The first complete
hardware profile is **EnvironLight** on the ESP32-S3-WROOM-1 N16R8. A separate
RadioScout target is present for the DOIT ESP32-WROOM-32D and is disabled by
default until that board is assembled and qualified.

## EnvironLight wiring

| Instrument | Connection |
|---|---|
| DHT22 | data GPIO 4 |
| BMP280 | SDA GPIO 6, SCL GPIO 7, address 0x76 or 0x77 |
| AS7341 | SDA GPIO 8, SCL GPIO 9 |
| ICS-43434 | SD GPIO 15, BCLK GPIO 16, WS GPIO 17 |

These are the known working EnvironLight connections carried into the new
architecture. Both I2C controllers initially run at the conservative 100 kHz
rate until the assembled board is qualified at a faster rate.

## What is implemented

- 48 kHz mono I2S acquisition with 24 significant bits in a 32-bit slot.
- 2,048-point Hann FFT with 512-sample hop, A and C weighting, sound-level
  statistics, octave-like bands, waveform envelope, spectrum and spectrogram.
- Explicit 5, 10 or 30 second RAM-only WAV capture and immediate download.
- AS7341 two-phase SMUX acquisition with adaptive gain and integration time.
- BMP280 compensated pressure, residual, slope, relative height and pressure
  spectrum; DHT22 climate readings at a 2.5 second interval.
- Passive BLE observation, bounded anonymous observations and a persistent
  registry of explicitly named devices. Named records can be archived and are
  never removed by an ordinary dashboard action.
- Bounded RAM and LittleFS history at 1 s, 10 s and 60 s resolutions, CRC
  recovery, event records, reference scenes and preservation of legacy files.
- Eight dashboard views: Observatory, Sound, Light, Atmosphere, Radio,
  Dynamics, Timeline and System.
- Local REST and WebSocket interfaces, responsive phone layout and the official
  Sun-Drunk Dawn and Sun-Drunk Dusk palettes.
- HMAC-authenticated two-slot OTA upload with image verification, a health grace
  period and rollback-enabled boot configuration on EnvironLight.

## Configure, build and flash

Install ESP-IDF 5.5.x, open its terminal, then run from this directory:

```sh
cd apps/environlight_s3
idf.py menuconfig
idf.py build
idf.py -p YOUR_PORT flash monitor
```

In `menuconfig`, set the Wi-Fi network and an API key of at least 16 characters.
Set a separate OTA signing key of at least 32 characters if browser firmware
updates are required; that key is used locally for HMAC and is never uploaded.
The dashboard remains readable when the key is blank, while mutations such as
capture, naming and provisioning stay disabled. Do not place passwords or API
keys in a source archive.

The normal local addresses are `http://environlight.local/` and the IP address
printed in the serial monitor. Existing Lenovo/Tailscale subnet routing can be
used for remote access; the firmware opens no public port and uses no cloud
service.

The release ZIP contains prebuilt binaries produced with blank credentials.
They are useful as build evidence and as a safe read-only starting image. Set
the configuration and rebuild before depending on control functions.

## Verification

Run the portable tests from the repository root:

```sh
./tools/test-host.sh
```

The suite exercises the sensor decoding and filters, DSP maths, binary stream
and WAV layout, radio registry lifecycle, history wrap/recovery and dashboard
math/protocol functions. Board tests remain essential for microphone offset,
light exposure behaviour, pressure cadence/noise and long-run radio coexistence.

## Deliberately deferred

Wake-word processing, transcription and clip playback are outside this release.
RadioScout does not claim CSI or channel hopping. The interface reports these
capabilities honestly through its manifest rather than simulating them.

The detailed product contract is in
`docs/specs/2026-10-15-environcore-v0.5-design.md`; component-specific hardware
validation notes are in `components/ec_sensors/VALIDATION.md`.
