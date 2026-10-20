# v0.5.0-dev1 release notes

This is the first integrated ESP-IDF build of the revised EnvironCore family
architecture and the first full EnvironLight profile.

The release includes separately keyed HMAC authentication for inactive-slot OTA,
image verification and health-gated boot rollback; device-owned Host and
same-origin protection for live WebSockets;
server-enforced BLE enrolment expiry, and aggregate-only anonymous BLE output.
Candidate identifiers rotate and appear only during an authorised enrolment
window; known-device names remain persistent as requested.

The source is ready for compiler and board qualification. Portable automated
tests cover the deterministic parts of the system. Values that depend on the
physical assembly remain labelled as estimates or pending calibration in the
API and dashboard.

Known commissioning work:

1. Measure the ICS-43434 offset against a stable reference and replace the
   provisional microphone sensitivity offset if needed.
2. Observe AS7341 saturation and settling flags under daylight, dim tungsten
   and coloured LED light.
3. Record BMP280 achieved cadence and noise with the assembled enclosure.
4. Run a 24-hour coexistence test with Wi-Fi, BLE, audio DSP and history active.
5. Check every dashboard view on the intended iPhone after the board is online.

The existing LittleFS partition is mounted without automatic formatting. If an
old or damaged filesystem cannot be mounted, live operation continues in RAM
and the firmware reports the storage fault instead of erasing the archive.
