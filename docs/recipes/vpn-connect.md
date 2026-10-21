---
id: recipes/vpn-connect
audience: author
intents: [vpn, connect vpn, change location, country, expressvpn]
tools: [list_tools, create_strategy, save_definition, create_release]
sources:
  - be:src/application/tools/network_flows.rs
  - conductor:missions/2026-10-07-strategy-sharing-every-user-creates-prompt-parts/RELIABILITY-QUEUE.md
verified_be: 92a0a9c1
verified_conductor: 732df11a
---
# Recipe — connect the phone's VPN to a location   (PROVEN, ExpressVPN only)

One AI step with the `connect_vpn` tool (`app`, `location`): opens ExpressVPN, finds the location, connects, and
succeeds only when the app shows Protected AND the requested location. It recovers from a leftover search screen and
can switch location while connected. One connect intent, then it waits — never tap Connect repeatedly.

Needs: ExpressVPN installed and signed in on the phone (Titan does not check the subscription). Other VPN apps are not
supported by this tool.

## Ask
"Which country or city? Is ExpressVPN installed and signed in on that phone?"
