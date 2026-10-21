---
id: devices/phone-locked
audience: both
intents: [phone locked, screen locked, asleep, after reboot, passcode]
tools: [device_detail, screen_thumb_jpeg]
sources:
  - agent:src/domain/device_facet_resolver.rs
  - fe:src/shared/i18n/locales/en/devices.json
verified_be: 92a0a9c1
verified_fe: a93e3f30
verified_agent: 2ca0430
---
# The phone screen is locked or asleep

You see: screen locked, asleep, or unknown. Unknown is not proof it is locked. A screenshot you read may be old — check
its age before believing it.

1. **User:** "Wake the iPhone and unlock it yourself, entering the passcode on the phone if asked. Leave it plugged in
   and tap Trust if it asks."
2. **User:** "Keep it awake while Titan checks it again."
3. **You:** look again and follow whatever is still missing.

After a restart an iPhone must be unlocked by hand once — nothing in Titan can type the passcode. Never ask the user
to tell you their passcode.
