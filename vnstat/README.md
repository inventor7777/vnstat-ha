# vnStat

Persistent `vnstat` traffic monitoring for Home Assistant, with an ingress dashboard and access to the real CLI output.

## What This App Does

- Runs `vnstatd` inside its own app container
- Stores the database in `/data/vnstat` so history survives restarts
- Exposes a simple ingress dashboard in Home Assistant
- Lets you inspect normal and JSON `vnstat` output from the web UI

## Raw CLI Access

If you want the real raw CLI from the Home Assistant host, the easiest pattern is:

```sh
docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" <command>
```

Common examples:

```sh
docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli
docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli -s
docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli -d
docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli -m
docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli --json
docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli -tr 2
```

Inside the container, you can also use:

```sh
vnstat-cli
```

That wrapper maps to:

```sh
vnstat --config /data/vnstat.conf
```

If you use the Home Assistant terminal often, you can also add a shell alias in your init commands:

```sh
echo 'alias vnstat='\''docker exec -it "$(docker ps --filter name=vnstat --format "{{.Names}}")" vnstat-cli'\''' >> /root/.zshrc
```
