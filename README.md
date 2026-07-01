# vnStat for Home Assistant

This repository contains a Home Assistant app that runs `vnstat` in its own persistent container instead of inside the temporary Home Assistant terminal environment.

The first version is intentionally simple:

- `vnstatd` stores its database in the app's `/data` directory so history survives app restarts and Home Assistant host reboots.
- A small ingress web UI shows live summaries pulled from `vnstat --json`.
- The app can auto-detect the primary network interface or let you pin one explicitly.

## Development image behavior

For local development and custom repositories, this app is currently configured to build locally from [`vnstat/Dockerfile`](/Users/Gabriel/GitHub%20&%20Projects/vnstat-ha/vnstat/Dockerfile) instead of pulling from GHCR.

That is why the `image:` key in [`vnstat/config.yaml`](/Users/Gabriel/GitHub%20&%20Projects/vnstat-ha/vnstat/config.yaml) is commented out right now.

If you later want published releases:

- publish multi-arch images to a registry such as GHCR
- uncomment `image:`
- tag releases so Home Assistant can pull versioned images instead of building locally

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

## CLI access inside the container

If you open a shell in the running app container, plain `vnstat` may still try its default database path and show an error.

Use the wrapper command instead:

```sh
vnstat-cli
vnstat-cli -d
vnstat-cli -m
vnstat-cli -s
```

If you want the raw command, this is the equivalent form:

```sh
vnstat --config /data/vnstat.conf
```

## Raw CLI from the HA host

From the Home Assistant host or Terminal app, the simplest pattern is:

```sh
docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli
```

Useful variants:

```sh
docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli -d
docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli -m
docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli --json
```

If you use the terminal often, you can also add an alias:

```sh
echo 'alias vnstat='\''docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli'\''' >> /root/.zshrc
```

## Next likely improvements

- Expose a lightweight JSON endpoint suitable for a future HA integration
- Add optional sensors for today's, this month's, and total traffic
- Add a simple "copy CLI command" helper for power users
