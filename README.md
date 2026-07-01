# vnStat for Home Assistant

This repository contains a Home Assistant app that runs `vnstat` in its own persistent container instead of inside the temporary Home Assistant terminal environment.

The first version is intentionally simple:

- `vnstatd` stores its database in the app's `/data` directory so history survives app restarts and Home Assistant host reboots.
- A small ingress web UI shows live summaries pulled from `vnstat --json`.
- The app can auto-detect the primary network interface or let you pin one explicitly.

## Why ingress first?

`vnstat` is best when it quietly collects data in the background. The cleanest way to view that inside Home Assistant is an ingress dashboard:

- it is available from the HA sidebar
- it does not depend on the Terminal app being installed
- it gives us room to add charts, exports, and entity bridging later

A CLI shortcut can still be added later, but it should be a convenience feature, not the primary interface.

## Repository layout

- `repository.yaml`: Home Assistant app repository metadata
- `vnstat/`: the actual app

## Install locally while developing

1. Open Home Assistant.
2. Go to `Settings` -> `Apps`.
3. Open the app store.
4. Use the three-dot menu and add this repository as a local/custom repository.
5. Install the `vnStat` app.

## Current behavior

- If `interface` is blank, the app tries the default-route interface first and then the first non-loopback interface.
- Data is persisted under `/data/vnstat`.
- The UI is available through ingress in the Home Assistant sidebar.

## Next likely improvements

- Expose a lightweight JSON endpoint suitable for a future HA integration
- Add optional sensors for today's, this month's, and total traffic
- Add a simple "copy CLI command" helper for power users
