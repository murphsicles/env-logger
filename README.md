# env-logger

[![@logging/env-logger](https://img.shields.io/badge/zorb-%40logging%2Fenv--logger-blue)](https://zorbs.io)

Environment-driven logging for Zeta, ported from [env_logger](https://github.com/rust-cli/env_logger).

Configure your log level via the `ZETA_LOG` environment variable — no code changes needed.

## Installation

```toml
[dependencies]
"@logging/env-logger" = "0.11.5"
```

## Quick Start

```zeta
// In your main.z
use env_logger;

fn main() {
    env_logger::init();
    log::info!("Application started");
    log::debug!("Debug info"); // only shown with ZETA_LOG=debug
}
```

Then run:

```
ZETA_LOG=debug zeta run main.z
```

## Usage

### Basic initialization

```zeta
// Simplest: reads ZETA_LOG env variable
env_logger::init();
```

### Builder pattern

```zeta
use env_logger::{Builder, Env, Target};

let env = Env::new("MY_LOG")
    .with_default_level(3) // default: info (3)
    .with_filter("my_app");

let builder = Builder::from_env(env)
    .target(Target::Stderr)
    .show_timestamps(true)
    .show_line_numbers(true);

builder.init();
```

### Try init (safe for repeated calls)

```zeta
if env_logger::try_init() {
    log::info!("Logger started");
}
```

## Environment Variables

| Variable | Values | Description |
|----------|--------|-------------|
| `ZETA_LOG` | `off`, `error`, `warn`, `info`, `debug`, `trace` | Set max log level |
| `ZETA_LOG_TARGET` | `stdout`, `stderr` | Output target |
| `ZETA_LOG_TIMESTAMPS` | `on`, `off` | Show/hide timestamps |

## Log Levels

| Level | Value | Description |
|-------|-------|-------------|
| Off | 0 | Disable all logging |
| Error | 1 | Critical errors |
| Warn | 2 | Warnings |
| Info | 3 | Informational messages (default) |
| Debug | 4 | Debug information |
| Trace | 5 | Detailed trace output |

## Dependencies

- `@std/log` — The Zeta log facade (required at runtime)

## License

MIT
