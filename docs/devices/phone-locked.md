---
id: devices/phone-locked
audience: both
intents: [phone locked, screen locked, asleep, after reboot, passcode]
tools: [device_detail, get_device_screen]
---
# The phone screen is locked or asleep

You see: screen locked, asleep, or unknown (the web app: "The phone is locked." / "The phone screen is asleep.").
Unknown is not proof it is locked. A cached screenshot may be old — to see the screen now take a fresh one with
`get_device_screen` (`control-the-phone.md`).

1. **User:** "Wake the iPhone and unlock it yourself, entering the passcode on the phone if asked. Leave it plugged in
   and tap Trust if it asks."
2. **User:** "Keep it awake while Titan checks it again."
3. **You:** look again and follow whatever is still missing.

After a restart an iPhone must be unlocked by hand once — the user unlocks it themselves. NEVER type the phone's
passcode (even though a text-typing phone action exists) and never ask the user to tell you their passcode.
