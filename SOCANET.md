# wasmd for socanet (Cosmos SDK v0.55)

Branch `socanet-sdk055` is wasmd v0.70.5 ported to the stack socanet runs:

- Cosmos SDK v0.55.0, CometBFT v0.40.0, `cosmos-sdk/store/v2` v2.0.0, Go 1.26.10;
- IBC from `github.com/lfdt-ibc/ibc-go/v11` v11.2.1-rc.2, the new module path of ibc-go, which targets SDK v0.55.

No official wasmd release supports SDK v0.55 yet (v0.70.5 targets v0.54). The changes:

1. The dependency bumps above, and the ibc-go imports rewritten to the `lfdt-ibc` path.
2. The changes of CosmWasm/wasmd PR #2575 ("add support for sdk v0.55", unmerged). The main ones:
   - x/params and x/protocolpool are gone in SDK v0.55, so the legacy x/params migration (`x/wasm/migrations/v2`, consensus version 2 to 3) now returns an error. A chain still on that version must upgrade through wasmd v0.70.4 first. socanet starts from genesis at the latest version, so it never runs it.
   - `NewAppModule` and `NewMigrator` lose the legacy subspace argument.
   - A nil-response check is added on migrate.
   - The test helpers and the example app follow the SDK changes.

Checked with `go vet ./x/... ./app/...` and `go test -tags muslc ./x/...` (all pass). socanet pins
this branch by commit with a `replace` in its `go.mod`. Switch back to upstream once wasmd
releases SDK v0.55 support: the store layout is unchanged.
