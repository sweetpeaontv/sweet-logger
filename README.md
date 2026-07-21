# <img src="sweet-logger.png" alt="" width="64"> Sweet Logger
## hey - sweet logger you got there!

A Godot 4 addon that makes reading logs and debugging sane when you run multiple game instances at once.

It's meant as a drop-in/stand-in for `print()`: same habit of sprinkling logs through your systems, but every line carries more context - which peer printed it, what kind of log it is, which script and function it came from. That makes it much easier to follow what each client (and the server) is doing when several instances share one console.

When you're testing multiplayer locally (host + clients in separate run instances), the editor console fills with interleaved output from every peer. Sweet Logger formats each line the same way so you can tell at a glance **what kind of log it is**, **which peer it came from**, **when it happened**, and **which script/function** produced it.


## Features

- **Peer-aware labels** - tags each line as `SERVER`, a client peer ID, or `DISCONNECTED`
- **Color-coded peers** - consistent colors per peer so interleaved console output is easy to scan
- **Log levels** - `log`, `info`, `warning`, `error`, `debug` with distinct colors
- **Script context** - optional script and function name columns
- **Local timestamps** - `mm:ss:ms` by default (optional hours)
- **Rich console output** - uses `print_rich` with aligned columns


## Installation

### From the Godot Asset Library

1. In Godot, open the **AssetLib** tab.
2. Search for **Sweet Logger** and download it.
3. Open **Project → Project Settings → Plugins** and enable **Sweet Logger**.
4. The plugin registers a `SweetLogger` autoload automatically.

### Manual

1. Copy the `addons/sweet-logger` folder into your project's `addons/` directory.
2. In Godot, open **Project → Project Settings → Plugins** and enable **Sweet Logger**.
3. The plugin registers a `SweetLogger` autoload automatically.


## Usage

Call the autoload from anywhere:

```gdscript
SweetLogger.info("Player joined", [], "lobby.gd", "on_peer_connected")
SweetLogger.warning("High latency: {0}ms", [rtt], "net.gd", "_process")
SweetLogger.error("RPC failed", [], "sync.gd", "apply_state")
SweetLogger.debug("Tick {0}", [tick], "game.gd", "_physics_process")
SweetLogger.log("Hello from peer")
```

### Message formatting

Pass optional args and use `{0}`, `{1}`, … placeholders:

```gdscript
SweetLogger.info("Spawned {0} at {1}", [entity_name, position], "spawner.gd", "spawn")
```

### API

| Method                                                                | Level   |
| --------------------------------------------------------------------- | ------- |
| `SweetLogger.log(message, args=[], script_name="", function_name="")` | General |
| `SweetLogger.info(...)`                                               | Info    |
| `SweetLogger.warning(...)`                                            | Warning |
| `SweetLogger.error(...)`                                              | Error   |
| `SweetLogger.debug(...)`                                              | Debug   |


All methods share the same signature: message, optional format args, optional script name, optional function name.

## Configuration

On the `SweetLogger` autoload (or in the inspector when selected):


| Export                 | Default | Description                             |
| ---------------------- | ------- | --------------------------------------- |
| `SHOW_SCRIPT_NAME`     | `true`  | Include the script name column          |
| `SHOW_FUNCTION_NAME`   | `true`  | Include the function name column        |
| `SHOW_TIMESTAMP_HOURS` | `false` | Use `hh:mm:ss:ms` instead of `mm:ss:ms` |

## Log line layout

Each line is roughly:

```
[LEVEL][timestamp][PEER][script::function] message
```

- **LEVEL** - color by type (info blue, warning yellow, error red, etc.)
- **timestamp** - local time for correlating events across instances
- **PEER** - `SERVER`, numeric client ID, or `DISCONNECTED`, each with a stable color
- **script::function** - where the log was emitted (when you pass those args)

## Why this exists

Godot's "Run Multiple Instances" is great for testing multiplayer games on one machine, but plain `print()` output from every peer lands in one console with no clear ownership. Sweet Logger keeps a fixed column layout and peer coloring so you can follow one client's story (or the server's) without losing the others.

## License

MIT - see [LICENSE](addons/sweet-logger/LICENSE).
