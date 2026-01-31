# Svelte 5 Acorn Extensions

Svelte 5 introduces a ground-up rewrite of its compiler. At its core,
it uses **Acorn** to parse JavaScript. To support Svelte-specific
syntax like **Runes** and **Snippets**, Svelte extends Acorn with
custom plugins.

## 1. Overview of Extensions

Svelte 5 doesn't just parse standard ECMAScript; it must handle
special "compiler instructions" and template-specific logic that
reside within `<script>` blocks or `.svelte.js` files.

### Key Extensions

* **Runes Support**: Parsing `$state`, `$derived`, etc., as
    first-class primitives.
* **TypeScript Integration**: Svelte uses `@sveltejs/acorn-typescript`
    to handle TS syntax within components.
* **Expression Parsing**: Enhanced handling of expressions inside
    template tags (e.g., `{@render ...}`).

---

## 2. Supported New Tokens

While Acorn typically follows the ESTree spec, Svelte 5 recognizes
specific patterns and "function-like" symbols. Strictly speaking,
most Runes are parsed as standard `Identifier` or `CallExpression`
nodes, but the compiler's extended parser treats them as
**Reserved Internal Symbols**.

| Token/Symbol | Description |
| :--- | :--- |
| `$state` | Declares reactive state. |
| `$derived` | Declares a reactive derivation. |
| `$effect` | Declares a side effect. |
| `$props` | Handles component properties. |
| `$bindable` | Marks a prop as bindable. |
| `$inspect` | Trigger for the compiler debugger. |
| `$host` | Accesses the custom element host. |
| `render` | Used in `{@render ...}` tags. |
| `snippet` | Used in `{#snippet ...}` blocks. |

---

## 3. Implementation Details & Github Links

The Svelte 5 parser logic is located primarily within the
`src/compiler/phases/1-parse` directory of the main repository.

### Core Parser Extension

The main entry point where Acorn is invoked and extended.

* [**compiler/phases/1-parse/index.ts**](https://github.com/sveltejs/svelte/blob/main/packages/svelte/src/compiler/phases/1-parse/index.ts)

### Svelte-TypeScript Plugin

Svelte's custom Acorn plugin for high-performance TS parsing.

* [**acorn-typescript Repository**](https://github.com/sveltejs/acorn-typescript)
* [**Plugin Implementation**](https://github.com/sveltejs/acorn-typescript/blob/main/src/index.js)

### JavaScript Expression Parser

Handles how Svelte scripts and expressions are processed via Acorn.

* [**read_script.ts**](https://github.com/sveltejs/svelte/blob/main/packages/svelte/src/compiler/phases/1-parse/read/script.ts)
* [**read_expression.ts**](https://github.com/sveltejs/svelte/blob/main/packages/svelte/src/compiler/phases/1-parse/read/expression.ts)

---

## 4. Why Acorn?

Svelte chooses Acorn because it is:

1. **Small & Fast**: Minimizes compiler overhead.
2. **Plugin-based**: Allows Svelte to "inject" logic into the
    tokenizer and parser without a full fork.
3. **Extensible**: Easily handles the `ecmaVersion: 'latest'`
    to stay current with JS standards.
