# `loc` and `range`, conventions for AST positioning

## Convention used by AST Explorer

[AST Explorer](https://astexplorer.net/) primarily displays node positions using
`loc` (line/column),
which is 1-based for lines and 0-based for columns in Acorn. The `range`
property is also available but less prominently used in the UI.

AST Explorer uses `loc` when present. It will also show `range` if the
parser includes it, but `loc` is the main one it expects for source
highlighting.

## Svelte Playground AST

Based on the internal structure of the Svelte compiler used in the
official playground at `svelte.dev`, the AST uses both character-based
offsets and line/column-based location objects.

## 1. Character-Based Offsets (`start` and `end`)

These are the most direct way the compiler tracks a node's position. They
represent the character index in the raw source string.

- `start`: The index of the first character of the node (0-based).
- `end`: The index of the character immediately following the node.
- Purpose: Used for high-performance string slicing and internal logic
  where the compiler needs to know exactly which byte range it is
  transforming.

## 2. The `loc` Object

The `loc` (Location) property provides a human-readable coordinate system
that corresponds to what you see in your code editor.

- Structure:
  - `loc.start`: Contains `line` (1-based) and `column` (0-based).
  - `loc.end`: Contains `line` (1-based) and `column` (0-based).
- Purpose: Essential for helpful error messages (e.g., "Error on line 5,
  column 10") and for mapping the generated code back to the original
  source.

## Data Structure Comparison

- `start`/`end`: Format `Number`. Example `start: 42`. Best for
  programmatic string manipulation.
- `loc`: Format `Object`. Example `{ line: 2, column: 5 }`. Best for UI
  feedback and debugging.

## Example AST Node Representation

If you were to look at a simple `<div>` tag in the Svelte Playground AST
viewer, the positioning data would look like this:

```json
{
  "type": "RegularElement",
  "name": "div",
  "start": 0,
  "end": 11,
  "loc": {
    "start": {
      "line": 1,
      "column": 0
    },
    "end": {
      "line": 1,
      "column": 11
    }
  },
  ...
}
```

## Notes on AST/CST Projection and Ranges

A projectional editor may present an AST in a layout that does not match
the original source code used to build it.

The nodes of traditional AST typically exclude inter-token whitespace. As a result, the union
of node ranges does not include all characters from the original source.
