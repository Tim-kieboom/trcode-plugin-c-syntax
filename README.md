# C/C++ syntax for TerminalCode

Syntax highlighting for C and C++ (`.c`, `.h`, `.cc`, `.cpp`, `.cxx`, `.c++`, `.hh`, `.hpp`, `.hxx`, `.h++`, `.ipp`, `.inl`) as a [TerminalCode](https://github.com/Tim-kieboom/TerminalCode) plugin.

It is a language and nothing else: there is no code in it and no pane. The plugin is

- `cpp.wasm`, the tree-sitter grammar of C++ (it parses C too) built for WebAssembly,
- `manifest.toml`, which says which files are C or C++ (`[language]`: `extensions`, `comment`) and has the
  highlight query as text.

TerminalCode loads the grammar in its sandbox (plugin API 1.12, TerminalCode 0.11.0 or later) and colors the
files with the query, using the colors of your theme (`syntax.keyword`, `syntax.string`, ... in the theme file).

## Use

"Plugins: Manage", install **C/C++**, and turn it on. C and C++ files are colored when they are opened, and
`ctrl+/` comments a line with `//`.

## Where the grammar comes from

The grammar is [tree-sitter/tree-sitter-cpp](https://github.com/tree-sitter/tree-sitter-cpp) at v0.23.4 (f41e1a0)
(MIT, `LICENSE-GRAMMAR`). It extends the C grammar, so `.c` and `.h` files parse with it too (a plugin has one grammar;
C code that uses a C++ keyword as a name, like `new` or `class`, may be colored a little off). The module was built
from that revision with the tree-sitter CLI:

```sh
git clone https://github.com/tree-sitter/tree-sitter-cpp && cd tree-sitter-cpp
git checkout v0.23.4
tree-sitter build --wasm -o cpp.wasm
```

A new version of the grammar needs a new `version` in `manifest.toml` and a release (the module is the file the
installer checks). The query starts from the `queries/highlights.scm` of tree-sitter-c and tree-sitter-cpp with its patterns in the opposite
order (TerminalCode takes the first of two patterns on the same text; the grammar's file is written for the last),
and adds control keywords, more operators, brackets, parameters, enumerators, macros and escapes, with capture
names that themes have colors for.

## Licence

The plugin is MIT or Apache-2.0, as the other plugins; the grammar is MIT (`LICENSE-GRAMMAR`).
