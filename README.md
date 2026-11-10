# C syntax for TerminalCode

Syntax highlighting for C (`.c`, `.h`) as a [TerminalCode](https://github.com/Tim-kieboom/TerminalCode) plugin.

It is a language and nothing else: there is no code in it and no pane. The plugin is

- `c.wasm`, the tree-sitter grammar built for WebAssembly,
- `manifest.toml`, which says which files are C (`[language]`: `extensions`, `comment`) and has the
  highlight query as text.

TerminalCode loads the grammar in its sandbox (plugin API 1.12, TerminalCode 0.11.0 or later) and colors the
files with the query, using the colors of your theme (`syntax.keyword`, `syntax.string`, ... in the theme file).

## Use

"Plugins: Manage", install **C**, and turn it on. C files are colored when they are opened, and
`ctrl+/` comments a line with `//`.

## Where the grammar comes from

The grammar is [tree-sitter/tree-sitter-c](https://github.com/tree-sitter/tree-sitter-c) at v0.24.1 (7fa1be1)
(MIT, `LICENSE-GRAMMAR`). The module was built from that revision with the tree-sitter CLI:

```sh
git clone https://github.com/tree-sitter/tree-sitter-c && cd tree-sitter-c
git checkout v0.24.1
tree-sitter build --wasm -o c.wasm
```

A new version of the grammar needs a new `version` in `manifest.toml` and a release (the module is the file the
installer checks). The query starts from the grammar's `queries/highlights.scm` with its patterns in the opposite
order (TerminalCode takes the first of two patterns on the same text; the grammar's file is written for the last),
and adds control keywords, more operators, brackets, parameters, enumerators, macros and escapes, with capture
names that themes have colors for.

## Licence

The plugin is MIT or Apache-2.0, as the other plugins; the grammar is MIT (`LICENSE-GRAMMAR`).
