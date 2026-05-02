# Lumencast

> **The missing standard for server-driven displays.**
>
> A wire protocol, a typed schema and a multi-runtime client kit, Apache 2.0, multi-language, broadcast-grade and IoT-grade.

A serveur authoritatif détient un état temps-réel. Il doit le **pousser** à un display passif (browser, kiosque natif, OBS browser source, iframe, terminal, mobile) qui le rend visuellement. Le display n'a aucune logique métier, ne mute pas le state local, ne fait aucun calcul. Il rend, point.

This pattern is everywhere — broadcast overlays, trading dashboards, NOC walls, esports brackets, ATC consoles, industrial SCADA, live betting tickers, conference Q&A boards, digital signage, game spectator UIs — and is **systematically reimplemented ad-hoc** in every domain. Lumencast extracts the kernel as :

- **LSDP/1** — *Leaf State Delta Protocol* — a WebSocket protocol that pushes typed state at leaf-grain (`path → value`), with snapshot+delta, sequenced delivery, gap detection, reconnect, and token rotation
- **LSML 1.0** — *Lumencast Scene Markup Language* — a JSON content-addressed format describing a tree of 8 closed primitives (`stack`, `grid`, `frame`, `text`, `image`, `shape`, `media`, `repeat`), with bindings, GPU-only animations, and operator-input metadata
- **Multi-language** — TypeScript / Go / Rust / Python — cross-paradigm, conformance-suite-tested

## Why a standard

The "real-time display" pattern is everywhere yet there is no shared protocol :

| Tool | What it gives | What it doesn't |
|---|---|---|
| Phoenix LiveView | HTML diffs server-pushed | Locked to Elixir/Phoenix. Pushes HTML, not state |
| Hotwire / Turbo Streams | HTML fragments | Locked to Rails. HTML, no schema |
| HTMX + SSE | DOM patches via SSE | DOM-only, no typed schema, large XSS surface |
| Liveblocks | Collaborative state | Bidirectional CRDT. Overkill for displays. SaaS |
| PartyKit | WebSocket-as-a-service | Pubsub generic, no rendering contract |
| Apollo subscriptions | GraphQL push | Typed but no rendering protocol |
| Caspar CG | Industry broadcast graphics | C++ from 2007, not web stack |
| Singular.live, Vizrt | SaaS broadcast graphics | Closed, vendor lock, expensive |

Lumencast fills the gap : an open protocol + schema you self-host, multi-language, broadcast-grade and dashboard-ready.

## The 3 invariants

1. **Push leaf-grain** — state travels as `path → value` patches at the leaf level. The client maintains `path → signal`. Only components reading that path re-render. No DOM diff. No reconciliation.
2. **Closed primitive catalog** — the runtime renders only the 8 LSML primitives. No `eval`, no `dangerouslySetInnerHTML`, no arbitrary HTML. **Security by construction.**
3. **Content-addressed scene bundle** — the bundle's identity is its sha256 hash. Cache forever. Multiple versions co-exist transparently. Rollback is just an URL change.

## Repos

| Repo | Purpose | Status |
|---|---|---|
| [`lumencast-protocol`](https://github.com/Lumencast/lumencast-protocol) | LSDP/1 + LSML 1.0 specs, conformance fixtures + scenarios, JSON Schema | **draft** — usable as reference |
| [`lumencast-js`](https://github.com/Lumencast/lumencast-js) | TypeScript runtime + Node server SDK monorepo | scaffolding |
| [`lumencast-go`](https://github.com/Lumencast/lumencast-go) | Go server SDK + `lumencast` CLI binary | scaffolding |
| [`lumencast-rs`](https://github.com/Lumencast/lumencast-rs) | Rust SDK | scaffolding |

More languages and runtimes (Python, Vue, Svelte, Flutter, TUI, native) land in subsequent waves once the wave 1 trio stabilizes.

## Status

This project is **early**. Specs are draft, no SDK has shipped a v0 release yet. The conformance suite ships v0 with 16 scenarios covering the LSDP/1 + LSML 1.0 contract surface.

Watch the org for releases — when wave 1 SDKs land their initial v0.1.0, this README will reflect it.

## Spec docs

- [LSDP/1 — wire protocol](../../lumencast-protocol/blob/main/spec/LSDP-1.md)
- [LSML 1.0 — scene format](../../lumencast-protocol/blob/main/spec/LSML-1.md)
- [Runtime API contract](../../lumencast-protocol/blob/main/spec/RUNTIME-API.md)
- [Performance budgets](../../lumencast-protocol/blob/main/spec/PERFORMANCE.md)
- [Error code taxonomy](../../lumencast-protocol/blob/main/spec/ERROR-CODES.md)
- [Conformance suite](../../lumencast-protocol/blob/main/conformance/README.md)

## Governance & contributing

- [Contributing guide](../../lumencast-protocol/blob/main/CONTRIBUTING.md)
- [Governance model](../../lumencast-protocol/blob/main/GOVERNANCE.md)
- [Code of conduct](../../lumencast-protocol/blob/main/CODE_OF_CONDUCT.md)
- [Security policy](../../lumencast-protocol/blob/main/SECURITY.md)
- [Decision log](../../lumencast-protocol/blob/main/DECISIONS.md)
- [RFC process](../../lumencast-protocol/blob/main/RFC-PROCESS.md)

## License

[Apache 2.0](../../lumencast-protocol/blob/main/LICENSE) across the entire ecosystem.

The project name "Lumencast" and the LSDP/LSML acronyms are trademarked separately ; only conformance-passing implementations may use the name without qualification (see [GOVERNANCE.md § Brand](../../lumencast-protocol/blob/main/GOVERNANCE.md#brand)).
