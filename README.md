# NimJS

**Proposed product:** a small, HTML-first framework for adding interaction to
server-rendered pages. Keep the HTML that already works; attach behavior only
where it is needed. NimJS should give plain JavaScript and TypeScript projects
a predictable controller lifecycle without requiring a virtual DOM or a
framework-specific template language.

**Status:** concept and architecture only. This repository currently has no
runtime, package, or working CLI. The API below is a design sketch.

## The problem

Many pages need a menu, filter, counter, form enhancement, or live status area,
not a full client-rendered application. Ad hoc event handlers are easy to start
but become hard to clean up and coordinate as the page changes. NimJS aims to
make those small interactions explicit and testable.

## Target experience

```html
<section data-nim-controller="counter">
  <output data-nim-target="count">0</output>
  <button type="button" data-nim-action="increment">Add one</button>
</section>
```

```ts
import { createApp } from '@nimjs/core';

const app = createApp();

app.controller('counter', ({ element, on }) => {
  const output = element.querySelector('[data-nim-target="count"]');
  let count = 0;

  on('click', '[data-nim-action="increment"]', () => {
    count += 1;
    if (output) output.textContent = String(count);
  });
});

app.start(document);
```

The intended runtime owns event listener cleanup when a controller root leaves
the page. The HTML remains useful before JavaScript loads. The exact API and
attribute names should be validated with a first implementation and example.

## Principles

- HTML is the source of page structure and accessible semantics.
- JavaScript adds local behavior inside an explicit controller root.
- Importing the package is safe during server rendering; DOM access happens at
  `start` time.
- Lifecycle and cleanup are automatic and testable.
- The core stays small; routing, data fetching, and rendering adapters are
  optional future packages, not prerequisites.

## NimJS family

- **NimJS:** HTML-first interaction runtime proposed here.
- **[UI](https://github.com/nimjs/ui):** React components and a component
  registry. It stays independently usable.
- **[Nim Lint](https://github.com/nimjs/lint):** independent code-quality tool.

Shared branding does not imply runtime dependencies between the projects.

## Project documents

- [Architecture](docs/architecture.md): runtime model and module boundaries.
- [Roadmap](docs/roadmap.md): build order and success criteria.
- [AI task briefs](docs/ai-tasks.md): focused implementation prompts.
- [Agent instructions](AGENTS.md): conventions for automated contributors.
