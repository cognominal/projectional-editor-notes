# Intended dev path

The goal is to develop an ecosystem around a projectional editor that
support push projections.
A [glossary](/GLOSSARY.md) explains

In the sveltekit route `/editor`, I am developing a language studio
to help the implementation of a projectional editor, `lushed`
eventually driven by language specific driver.

The projectional editor will eventually be the main pane
of a tool like vscode.

## End goal

Having a site modelled after [svelte.dev](https://svelte.dev) with a playground
using a `lushed` pane.

## yaml as idempotent projection

I am starting by yaml using an idempotent projection,
meaning the projection unstyled text is the original
yaml.
Lyaml is an extended yaml that includes js vars.
That makes a [Two-level Embedding](/GLOSSARY.md#two-level-embedding-enhanced-yaml).
Also yaml will be used to specify complex literal as
embedding in the posh  projection of an extended typescript.

The posh projection of typescript
In posh projection of language, especially when using indentation as
syntax.
non space structural tokens tends
to disappear.
