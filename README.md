# NTv2 — Universal GVG K-Frame CPL Bridge (`V100`)

A Python WebSocket-to-UDP bridge for Grass Valley (GVG) K-Frame video switchers (compatible with firmware `14.7.1` – `20.1.1`). It implements the full GVG UDP handshake, dynamic port negotiation, heartbeat supervision, and real-time TCP ETP tally parsing.
Serial 422 Editor mode is for future implementation.
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
To run, use Connect100.bat which will open a python gvg bridge. On the GVG Frame, add the ip of the Bridge in two places, under node control panel and ip port for tally.
Inside the batch file Connect100,bat you can edit the flag as needed.
Flag,Default,Description

--ip,127.0.0.1 (default) ,IP address or hostname of the GVG K-Frame.

--suite,1,"Target switcher suite (1, 2, 3, or 4)."

--ws-host,0.0.0.0,Local WebSocket bind address.

--ws-port,8765,Local WebSocket bind port.

--live,False,Enable live UDP/TCP communication with the frame (omit for dry-run).

--name,NTv2,"Base panel name shown in K-Frame connections (<name> - Suite N, max 24 ASCII chars)."

--ports,Auto,"Override local UDP MAIN:LISTENER ports per suite (e.g., 1=6130:6131,2=6132:6133). Defaults to 6130/6131 for Suite 1, incrementing by 2 per suite."

--fps,30.0,"Frame rate used to convert frame-based rate commands (""frames"": n) to seconds." 23.976/24, 25/50, 29.97/30, and 59.94/60.

--version 20.1.1 (default) This flag allow the bridge to provide a match version with your GVG frame.

Ask Nava if you lost...My recommendation is to push the file to your prefered AI connector and have it made you a custom web ui base on your needed.
Your AI should be able to find all the "control" to control to GVG frame 

## Requirements

* Python 3.8+
* `websockets` library:
  ```bash
  pip install websockets
