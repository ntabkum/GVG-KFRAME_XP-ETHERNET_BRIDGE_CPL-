# NTv2 — Universal GVG K-Frame CPL Bridge (`V100`)

A Python WebSocket-to-UDP bridge for Grass Valley (GVG) K-Frame video switchers (compatible with firmware `14.7.1` – `20.1.1`). It implements the full GVG UDP handshake, dynamic port negotiation, heartbeat supervision, and real-time TCP ETP tally parsing.

---

## Features

* **Full GVG UDP Session State Machine**: Executes the two-stage UDP handshake (`5000` / `5001` port announcement -> dynamic comms port), suite selection (`Suite 1`–`4`), packet acknowledgments, and 1-second heartbeat keep-alives with automatic exponential back-off reconnection.
* **5 M/E Bus Support**: Controls and monitors `ME1` (`0`), `ME2` (`1`), `ME3` (`2`), `PGM` (`3`), and `ME4` (`4`).
* **Switcher Control**:
  * **Crosspoints**: Route sources (`1–809`) to A (`pgm`) and B (`pvw`) buses.
  * **Transitions**: Trigger `CUT` and `AUTO` transitions per M/E.
  * **AUX Routing**: Direct source takes on AUX buses `1–48`.
  * **Transition & Keyer Rates**: Set durations in seconds (`0–15s`) or frames (auto-converted via `--fps`).
  * **Macro Playback**: Queued execution of macros `1–999` with frame acknowledgment tracking.
* **Dual Real-Time Feedback**:
  * **TCP ETP Tally (`Port 2012`)**: Streams live XML tally updates (`VPEInputContribution` / `VPEOutputContribution`) to resolve actual on-air `PGM` and `PVW` sources.
  * **UDP ME-Bus Subscriptions**: Subscribes directly to A/B bus states across all 5 M/E addresses.
* **Safe Dry-Run Mode**: Runs without `--live` by default to validate JSON payloads and log commands without transmitting packets to the switcher.

---

## Requirements

* Python 3.8+
* `websockets` library:
  ```bash
  pip install websockets
