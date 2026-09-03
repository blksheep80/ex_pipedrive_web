# ExPipedriveWeb

[![Hex.pm](https://img.shields.io/hexpm/v/ex_pipedrive_web.svg)](https://hex.pm/packages/ex_pipedrive_web)
[![Hex Docs](https://img.shields.io/badge/hex-docs-lightgreen.svg)](https://hexdocs.pm/ex_pipedrive_web/)
[![CI](https://github.com/blksheep80/ex_pipedrive_web/actions/workflows/elixir.yml/badge.svg)](https://github.com/blksheep80/ex_pipedrive_web/actions/workflows/elixir.yml)
[![Coverage Status](https://coveralls.io/repos/github/blksheep80/ex_pipedrive_web/badge.svg?branch=main)](https://coveralls.io/github/blksheep80/ex_pipedrive_web?branch=main)

Optional Plug helpers for **incoming Pipedrive webhooks**. Payload
normalization (`ExPipedrive.Webhook.Event`) and the handler behaviour live in
core [`ex_pipedrive`](https://hex.pm/packages/ex_pipedrive)
([GitHub](https://github.com/blksheep80/ex_pipedrive)). This package owns
the Plug router only.

Phoenix is not a dependency: a Phoenix router can `forward` to the Plug the
same way a raw Plug router can.

## Installation

```elixir
def deps do
  [
    {:ex_pipedrive, "~> 0.2.0"},
    {:ex_pipedrive_web, "~> 0.1.1"}
  ]
end
```

Core stays usable without this package (API client, OAuth TokenStore, webhook
*subscription* CRUD). Add `ex_pipedrive_web` only if you mount an inbound
webhook endpoint.

## Usage

Implement the core behaviour:

```elixir
defmodule MyApp.PipedriveWebhookHandler do
  @behaviour ExPipedrive.Webhook.Handler

  @impl true
  def handle_event(%ExPipedrive.Webhook.Event{action: "updated", resource: "deal"} = event) do
    MyApp.Deals.handle_update(event.current, event.previous, event.diff)
  end

  def handle_event(_event), do: :ok
end
```

Forward traffic from Plug or Phoenix. Basic auth is optional (`:auth_fn`).

```elixir
forward "/webhooks", to: ExPipedriveWeb.Incoming.Handler,
  init_opts: [
    handler: MyApp.PipedriveWebhookHandler,
    auth_fn: fn ->
      [username: "pipedrive", password: System.fetch_env!("PIPEDRIVE_WEBHOOK_SECRET")]
    end
  ]
```

Existing mounts that use `ExPipedrive.Incoming.Handler` still work as
compatibility aliases once this package is in `deps`.

Legacy `on_event: fn {:updated_deal, payload} -> ... end` remains supported
for deal/person updates; new code should use `ExPipedrive.Webhook.Handler`.

The host application owns fan-out (Oban, PubSub, Registry). This package does
not start an OTP application or process mailbox.

## Version coupling

| This package | Core |
|---|---|
| `0.1.x` | `ex_pipedrive ~> 0.2` |

If you clone this repo next to [`ex_pipedrive`](https://github.com/blksheep80/ex_pipedrive),
Mix uses a path dependency on core. CI and Hex consumers use
`{:ex_pipedrive, "~> 0.2"}`.

To publish (when `../ex_pipedrive` exists locally):

```bash
HEX_PUBLISH=1 mix hex.publish
```

Hex: [`ex_pipedrive_web`](https://hex.pm/packages/ex_pipedrive_web).

## Development

Issue tracking: [beads](https://github.com/steveyegge/beads) via `bd` (prefix `expdw-`).
GitHub issues on this repo are the product backlog.

```bash
mix deps.get
mix test
mix coveralls
mix format --check-formatted
mix credo --strict
```

Coverage HTML: `mix coveralls.html` (opens `cover/excoveralls.html`). CI uploads lcov from the primary Elixir 1.17 cell to [Coveralls](https://coveralls.io/github/blksheep80/ex_pipedrive_web) and fails if total coverage drops below 60%.
