# LinkHarbor for Minecraft (Paper)

A Paper plugin that shows your Minecraft server on your [LinkHarbor](https://linkharbor.com) bio page and tells your followers when people join. Works on Paper, Purpur, Pufferfish and other Paper forks, Minecraft 1.20 or newer, Java 21.

On the page you get a game-server card: online or offline, "3 / 20" players, the names of who is on, the version, a copyable address and a button to your Discord or website. Your followers can get push notifications from rules you define, for example "{count} players joined: {player_list}" at most every ten minutes.

Part of the [LinkHarbor integrations](https://github.com/linkharbor/integrations) family, one repo per game. Running a Bedrock server? Use [integration-minecraft-bedrock](https://github.com/linkharbor/integration-minecraft-bedrock).

## Install

1. Download `LinkHarbor-<version>.jar` from the [releases](https://github.com/linkharbor/integration-minecraft/releases) and put it into `plugins/`.
2. Start the server once. The plugin writes `plugins/LinkHarbor/config.yml`.
3. In the LinkHarbor dashboard open Workspace, Integrations, Custom (API). Give it a name such as "Survival SMP" and copy the Ingest URL and the Secret.
4. Paste both into `config.yml`. Set `server.address` to what players type to join, and `server.url` to your Discord invite or website if you want a button on the card. Then run `linkharbor reload` in the server console, or restart the server.
5. Add an Integration block to your bio page and pick the integration.

`linkharbor status` shows the last response from LinkHarbor. `linkharbor push` sends the current state right away. In-game the commands take a leading `/` and need the `linkharbor.admin` permission, which operators have.

## Events sent

| Event | When | Template variables |
|---|---|---|
| `player.joined` | a player joins | `{player}`, `{players_online}`, `{players_max}`, `{name}`, `{players}` |
| `player.left` | a player leaves | same |
| `server.started` | the plugin is enabled | `{name}`, `{players_max}` |
| `server.stopped` | the plugin is disabled or the server stops | `{name}` |

Every event carries the full state. A state-only heartbeat goes out every `heartbeat_minutes` (default 10) so the card never goes stale.

Suggested push rule (dashboard, Integrations, Push rules, Custom (API), event `player.joined`): batch window 10 minutes, title "🎮 {count} players joined", body "{player_list} are now on {name}", minimum pause 5 minutes.

## Configuration

Every option is explained in [`src/main/resources/config.yml`](src/main/resources/config.yml). The file holds your secret, so keep it out of version control.

## Build from source

```bash
mvn -q package
```

The jar lands in `target/LinkHarbor-<version>.jar`. You need Maven and JDK 21. There are no runtime dependencies beyond the Paper API; Gson and the HTTP client ship with the server and the JDK. Pushing a tag like `v1.0.0` builds the jar in GitHub Actions and attaches it to the release.

## Privacy

Only what you see in `config.yml` leaves the server: player names (turn off with `send_player_names: false`), player counts, the version and your own address and URL. No UUIDs, IP addresses or chat. Requests go to the Ingest URL and nowhere else.

## License

[MIT](LICENSE). © 2026 EMI Solutions OÜ.
