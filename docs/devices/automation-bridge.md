---
id: devices/automation-bridge
audience: both
intents: [wda, automation bridge, wrong version, not signed, verification failed, cannot control phone]
tools: [device_detail]
sources:
  - agent:src/app/wda_lifecycle_fsm.rs
  - agent:src/app/wda_lifecycle_reinstall.rs
  - agent:src/domain/wda_lifecycle_resolution.rs
  - fe:src/shared/i18n/locales/en/devices.json
verified_be: 92a0a9c1
verified_fe: a93e3f30
verified_agent: 2ca0430
---
# The automation link (WDA) is not ready

You see: the automation bridge not ready, failed, wrong_version, not_signed, or the web app says "The reporting host
cannot confirm the WDA automation bridge". Say "the automation link on the phone".

1. **You:** `device_detail` — USB, trust, tunnel, developer disk and Developer Mode must be fine first. If the tunnel
   isn't ready, the link can't start: fix `tunnel-down.md` first.
2. **User:** "Keep the phone plugged in, unlocked and awake, and leave the PC Agent running while it sets up the
   automation link." Titan installs and starts it by itself; after a failure it waits about 30 seconds and tries again.
3. "Missing assignment": "Ask your Titan administrator to assign the automation build for this phone." Nobody else can
   do this.
4. `wrong_version` after an install means Titan installed it but could not confirm the app afterwards — not proof of a
   wrong number. `not_signed` also appears when trust isn't ready. Neither is a reason to reinstall anything by hand.
5. Still not ready after a few minutes: `collect-logs.md`.

Never suggest installing an app file by hand, erasing the phone, or re-pairing to fix this.
