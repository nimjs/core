# Instructions for coding agents

Read `README.md`, `docs/architecture.md`, and `docs/roadmap.md` before coding.
The runtime is a proposal until a working implementation and example exist.

- Keep HTML meaningful before JavaScript starts. Use native semantic elements
  in examples and preserve server-rendered content.
- Keep controller behavior local to its root. Define ownership for nested
  controllers and test it.
- Make listener and observer cleanup deterministic; test repeated start/stop.
- Avoid reading `window` or `document` at module import time.
- Do not introduce a renderer, router, compiler, React dependency, or UI
  dependency into the core without a documented product decision.
- Maintain an explicit, small public API and update examples when it changes.
- State which commands and capabilities exist today; label proposals clearly.
- Run focused tests for code changes and report what was verified.
