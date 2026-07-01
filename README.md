# vnStat for Home Assistant

- `vnstatd` stores its database in the app's `/data` directory so history survives app restarts and Home Assistant host reboots.
- A light ingress web UI shows data pulled from `vnstat --json`, and also exposes CLI output and the true CLI inside the container.
- The app can auto-detect the primary network interface or let you pin one explicitly.

## Accessing the true CLI

From the Home Assistant host or Terminal app, the simplest pattern is:

```sh
docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli
```

Useful variants:

```bash
docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli -d
docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli -m
docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli --json
```

If you use the terminal often, you can add this to your Terminal init commands to get native-like CLI behavior:

```bash
echo 'alias vnstat='\''docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli'\''' >> /root/.zshrc
```

Since this is the standard vnStat, all CLI commands should be available. To see all the commands, [see vnStat's official website.](https://humdi.net/vnstat/)

## Technical Info

- If `interface` is blank, the app's UI tries the default-route interface first and then the first non-loopback interface.
- Data is persisted under `/data/vnstat`.
- The UI is available through standard Ingress
- The vnStat image is very lightweight. To keep it simple, rather than pulling from GHCR, this image will automatically build when you install it.
