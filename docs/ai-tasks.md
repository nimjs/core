# AI task briefs

Use one brief at a time. The agent should read `README.md`,
`docs/architecture.md`, and `AGENTS.md` first.

## Runtime vertical slice

> Implement the smallest browser-usable NimJS runtime from the documented API
> sketch: `createApp`, controller registration, `start`, `stop`, delegated `on`,
> and `cleanup`. Add a static counter example. Test repeated lifecycle calls,
> nested roots, and SSR-safe import. Do not add routing, a virtual DOM, or
> framework adapters. Update the docs when the final API differs from the
> sketch.

## Lifecycle review

> Review controller connection and teardown for leaks and double connection.
> Create tests for moved nodes, nested controllers, removed subtrees, and
> event ownership. Report the smallest concrete fix for each failure.

## Developer experience

> Use the public NimJS package in a clean static site and a server-rendered
> example. Record every undocumented step, confusing name, and server-side
> import error. Fix the highest-impact gap and update getting-started docs.
