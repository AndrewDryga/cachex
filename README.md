# Cachex
[![Coverage Status](https://img.shields.io/coveralls/zackehh/cachex.svg)](https://coveralls.io/github/zackehh/cachex) [![Unix Build Status](https://img.shields.io/travis/zackehh/cachex.svg?label=unix)](https://travis-ci.org/zackehh/cachex) [![Windows Build Status](https://img.shields.io/appveyor/ci/zackehh/cachex.svg?label=win)](https://ci.appveyor.com/project/zackehh/cachex) [![Hex.pm Version](https://img.shields.io/hexpm/v/cachex.svg)](https://hex.pm/packages/cachex) [![Documentation](https://img.shields.io/badge/docs-latest-blue.svg)](https://hexdocs.pm/cachex/)

Cachex is an extremely fast in-memory key/value store with support for many useful features:

- Time-based key expirations
- Maximum size protection
- Pre/post execution hooks
- Statistics gathering
- Multi-layered caching/key fallbacks
- Transactions and row locking
- Asynchronous write operations
- Syncing to a local filesystem
- User command invocation

All of these features are optional and are off by default so you can pick and choose those you wish to enable.

## Table of Contents

- [Installation](#installation)
- [Getting Started](docs/getting-started.md)
    - [Starting Your Cache](docs/getting-started.md#starting-your-cache)
    - [Main Interface](docs/getting-started.md#main-interface)
- [Action Blocks](docs/action-blocks.md)
    - [Execution Blocks](docs/action-blocks.md#execution-blocks)
    - [Transaction Blocks](docs/action-blocks.md#transaction-blocks)
- [Cache Limits](docs/cache-limits.md)
    - [Configuration](docs/cache-limits.md#policies)
    - [Policies](docs/cache-limits.md#policies)
- [Custom Commands](docs/custom-commands.md)
    - [Defining Commands](docs/custom-commands.md#defining-commands)
    - [Invoking A Command](docs/custom-commands.md#invoking-a-command)
- [Disk Interaction](docs/disk-interaction.md)
- [Execution Hooks](docs/execution-hooks.md)
    - [Creating Hooks](docs/execution-hooks.md#creating-hooks)
    - [Provisions](docs/execution-hooks.md#provisions)
- [Fallback Caching](docs/fallback-caching.md)
    - [Common Use Cases](docs/fallback-caching.md#common-use-cases)
    - [Expirations](docs/fallback-caching.md#expirations)
    - [Handling Errors](docs/fallback-caching.md#handling-errors)
- [TTL Implementation](docs/ttl-implementation.md)
    - [Janitor Processes](docs/ttl-implementation.md#janitor-processes)
    - [On Demand Expiration](docs/ttl-implementation.md#on-demand-expiration)
- [Benchmarks](#benchmarks)
- [Contributions](#contributions)

## Installation

As of v0.8.0, Cachex is available on [Hex](https://hex.pm/). You can install the package via:

  1. Add cachex to your list of dependencies in `mix.exs`:

    ```elixir
    def deps do
      [{:cachex, "~> 2.1"}]
    end
    ```

  2. Ensure cachex is started before your application:

    ```elixir
    def application do
      [applications: [:cachex]]
    end
    ```

## Usage

The typical use of Cachex is to set up using a Supervisor, so that it can be handled automatically:

```elixir
Supervisor.start_link(
  [
    worker(Cachex, [:my_cache, []])
  ]
)
```

If you wish to start it manually (for example, in `iex`), you can just use `Cachex.start_link/2`:

```elixir
Cachex.start_link(:my_cache, [])
```

For anything else, please see the [documentation](https://github.com/zackehh/cachex/tree/master/docs).

## Benchmarks

There are some very trivial benchmarks available using [Benchfella](https://github.com/alco/benchfella) in the `bench/` directory. You can run the benchmarks using the following command:

```bash
# default benchmarks, no modifiers
$ mix bench

# use a state instead of a cache name
$ CACHEX_BENCH_STATE=true mix bench

# use a lock write context for all writes
$ CACHEX_BENCH_TRANSACTIONS=true mix bench

# use both a state and lock write context
$ CACHEX_BENCH_STATE=true CACHEX_BENCH_TRANSACTIONS=true mix bench
```

## Contributions

If you feel something can be improved, or have any questions about certain behaviours or pieces of implementation, please feel free to file an issue. Proposed changes should be taken to issues before any PRs to avoid wasting time on code which might not be merged upstream.

If you *do* make changes to the codebase, please make sure you test your changes thoroughly, and include any unit tests alongside new or changed behaviours. Cachex currently uses the excellent [excoveralls](https://github.com/parroty/excoveralls) to track code coverage.

```bash
$ mix test
$ mix credo
$ mix coveralls
$ mix coveralls.html && open cover/excoveralls.html
```
