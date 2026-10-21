---
id: devices/host-deps
audience: both
intents: [host deps, missing dependency, python missing, ffmpeg, wintun, screenshot service]
tools: [get_host_deps]
sources:
  - agent:src/domain/status_host_deps.rs
  - agent:src/app/host_dep_remediation_admit.rs
  - agent:src/app/host_deps_facets.rs
  - agent:docs/NEW-USER-WINDOWS-SETUP.md
verified_be: 92a0a9c1
verified_fe: a93e3f30
verified_agent: 2ca0430
---
# Host Deps shows something missing

The PC needs: python, pmd3, sidecar_deps, tunneld, screenshot_daemon, ffmpeg, and on Windows also amds (Apple Mobile
Device Support) and wintun. States: ok, missing, installing, install-failed, unknown (checking is separate).

1. **You:** `get_host_deps` — check the snapshot is fresh first (an offline agent shows its last known rows). Fix what
   a row is blocked on first.
2. **User:** "Open **Host Deps** in the left menu and use the button next to the item (**Re-scan**, **Re-verify**,
   **Install**, **Start** or **Repair** — whichever is shown)."
3. By system:
   - **Windows:** amds → install Apple Mobile Device Support (`usb-service-windows.md`). Missing python, pmd3,
     sidecar_deps or wintun on an installed Titan → "Reinstall Titan with the current installer." Never install
     Python or packages separately.
   - **Mac:** missing packaged parts → "Reinstall with the Titan installer." Never Homebrew Python.
   - **Linux:** use the command Host Deps shows for the distribution, or ask whoever set it up.
4. screenshot_daemon problems (`asset_missing` → reinstall; `restart_budget_exhausted` → collect logs before anything).
5. **You:** confirm the row is ok in a fresh report.
