# goNDK

[![Run Tests](https://github.com/niallyoung/goNDK/actions/workflows/main.yaml/badge.svg)](https://github.com/niallyoung/goNDK/actions/workflows/main.yaml)

goNDK is a NOSTR Development Kit in Golang

## Goals

- well-engineered framework of NOSTR types, funcs and interfaces
- 95%+ test coverage, for rapid prototyping and production-ready solutions at scale
- ensure the NOSTR development experience in Go is high-quality, productive, easy and fun

## Status

* initial `v0.0.x` Type build-out underway, commenced Easter 2024
* total test coverage = 96.97%+ (see `./.meta/cover.sh`)

- [x] `Event{}`
- [x] `Identity{}` **WIP**
- [x] `Client{}`, `RelayManager{}` **WIP**
  - [ ] `Subscription{}`, `SubscriptionFilter{}`
- [ ] `Relay{}`, `Transport{}`, `ClientManager{}`,
  - [ ] `Outbox{}`, `Inbox{}`

~v0.1.0 ETA: ~Jul/Aug 2024: functional `Client{}`
  * publishing to a public relay with successful downstream relay propagation:
  * cohesive types, moderately de-coupled
  * interfaces established, all messages via interfaces

~v0.2.0 ETA: ~Sep/Oct 2024: functional `Relay{}`
  * publishing to `Relay{}` with successful downstream relay propagation:
  * all dependencies injected, optional adapters to shim to interfaces
  * mocks covering all interfaces, refactor unit tests with injection

## Development

```shell
make lint     # golangci-lint
make test     # unit tests
make cover    # 95%+ coverage
make generate # code generation (event/event_easyjson.go)

make docker.build
make docker.lint
make docker.test
make docker.cover
make docker.shell
```

## Usage

### Installation

```shell
go get github.com/niallyoung/goNDK
```

### Event{}

```go
import (
    "github.com/niallyoung/goNDK/event"
)

// unmarshal an incoming JSON serialised Event
var event event.Event
err := json.Unmarshal([]byte(`{"kind": 1, "content": "...", ... }`), &event)

// create and sign a new event
e := event.NewEvent(1, "hello world!", event.Tags(nil), nil, nil, nil, nil)
err := e.Sign(privateKey.Key.String()) // Sign an Event

// serialization
text := e.String()
bytes := e.Serialize()

// validation
err := e.Validate()
ok, err := e.ValidateSignature()
```

### Identity{}

```go
import (
	"github.com/niallyoung/goNDK/identity"
)

i := identity.NewIdentity(pubkey, npub)
err := i.Validate()
```

### Client{}

```go
import (
	"github.com/niallyoung/goNDK/client"
)

c := client.NewClient()
err := c.Validate()
```

## Thanks

Built upon, around and inspired by:

* [khatru](https://github.com/fiatjaf/khatru)
* [go-nostr](https://github.com/nbd-wtf/go-nostr)
* [shota3506/go-nostr (client)](https://github.com/shota3506/go-nostr)
* [nostr-domain](https://github.com/dextryz/nostr-domain)

## License

MIT License
Copyright (c) 2024 Niall Young <5465765+niallyoung@users.noreply.github.com>