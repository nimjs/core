# Architecture

This is a **proposed design**, not a description of existing code.

## Product boundary

NimJS enhances HTML that can already be rendered by a server, static-site
generator, or template system. It does not own page routing, server rendering,
or the application's data source. The first release supports client-side
controller attachment and cleanup.

```text
existing HTML
  -> discover [data-nim-controller] roots
  -> instantiate a registered controller once per root
  -> attach scoped event listeners
  -> disconnect and clean up when the root is removed
```

This boundary keeps the core usable with plain HTML and with server-rendered
frameworks. A future adapter may integrate with a specific framework without
making that framework a core dependency.

## Planned source layout

```text
src/
  app/          Registration, start/stop, duplicate-controller handling
  dom/          Root discovery, scoped target lookup, event delegation
  lifecycle/    Connection state, disposers, mutation observation
  types/        Public controller context and error types
  index.ts      Explicit public API
examples/
  counter/     Static HTML example used as an integration fixture
docs/
  architecture.md
  roadmap.md
```

Begin as one package, proposed name `@nimjs/core`. Split packages only after
a second independently useful capability exists.

## Controller lifecycle

1. `createApp()` creates an isolated registry and runtime instance.
2. `app.controller(name, factory)` registers a factory. Duplicate names fail
   with an actionable error.
3. `app.start(root)` scans `root` for controller elements and connects each
   registered name exactly once per element.
4. A controller receives its root element and helpers for scoped listeners,
   targets, and cleanup callbacks.
5. A `MutationObserver` can connect newly inserted roots and disconnect removed
   roots. A removed subtree must release all listeners and observers owned by
   its controllers.
6. `app.stop()` disconnects everything and allows a later clean `start()`.

The first vertical slice may use explicit `refresh()` instead of mutation
observation. If so, document it clearly and add automatic observation only
after lifecycle tests are stable.

## Public API sketch

```ts
interface ControllerContext {
  element: HTMLElement;
  on(
    event: string,
    selector: string,
    handler: (event: Event, matched: Element) => void,
  ): void;
  target(name: string): Element | null;
  cleanup(disposer: () => void): void;
}

interface App {
  controller(name: string, factory: (context: ControllerContext) => void): void;
  start(root?: ParentNode): void;
  stop(): void;
}

declare function createApp(): App;
```

`on` uses event delegation within the controller root. Nested controllers
must not receive another controller's actions by accident. `target` searches
only inside its owner's root. These ownership rules need explicit tests.

## Rendering and state

The core does not render HTML. Controllers update DOM nodes they own. A small
signal or state helper may be added later if repeated examples show a clear
need; its update timing and cleanup semantics must be specified first. Do not
make server HTML disappear or require a hydration payload just to attach an
event handler.

## Accessibility and resilience

- Examples use native elements and work as meaningful HTML before startup.
- Controllers must not replace buttons or links with non-semantic click targets.
- A failed controller should report its name and element while allowing other
  roots to connect; error-handling policy must be tested before release.
- No browser globals are read at module import time. `start()` is the DOM
  boundary and may require a root in non-browser environments.

## Package and compatibility

Publish an ESM package with TypeScript declarations and explicit exports.
Set a browser support policy and package name before release. Keep the public
core free of React, router, bundler, and UI package dependencies. Measure size
and startup cost on a real page before making performance claims.
