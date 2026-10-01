# Roadmap

## 0. Validate the concept

- Build a static HTML counter and one realistic interaction, such as a menu or
  filter, without a build step.
- Write down what HTML does before JavaScript starts and after it fails.
- Review API and attribute names against the examples before calling them stable.

**Done when:** the concept can be explained in one paragraph and the examples
show a clear advantage over loose event listeners.

## 1. Controller runtime

- Add package tooling, public types, `createApp`, registration, `start`, and
  `stop`.
- Implement scoped event listeners and deterministic cleanup.
- Test duplicate registration, repeated start/stop, nested roots, and SSR-safe
  import.

**Done when:** the counter example runs in a browser and its listeners disappear
after `stop()`.

## 2. Dynamic DOM

- Add insertion and removal observation or an explicit `refresh` protocol.
- Avoid double connection when nodes move within the observed tree.
- Test nested controller ownership and teardown of removed subtrees.

**Done when:** dynamically inserted HTML behaves the same as initial HTML.

## 3. First release

- Publish installation and migration-free getting-started documentation.
- State browser support, package size, and compatibility guarantees.
- Verify package name, license, CI, and release process.
- Test an external static site and one server-rendered app.

**Done when:** both consumers can install and use only the documented public
API, with no repository-internal imports.

## Later, only if examples justify it

Lazy controller loading, a tiny state primitive, form helpers, and framework
adapters. Routing and a compiler are outside the initial product boundary.
