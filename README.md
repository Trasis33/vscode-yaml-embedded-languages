<div align="center">

# YAML Embedded Languages (Zed)

![GitHub License](https://img.shields.io/github/license/harrydowning/yaml-embedded-languages?style=for-the-badge)

</div>

A [Zed](https://zed.dev) editor extension that adds syntax highlighting
**inside YAML block-scalars** for 50+ languages. This is the Zed port of the
original [`harrydowning.yaml-embedded-languages`](https://marketplace.visualstudio.com/items?itemName=harrydowning.yaml-embedded-languages)
VS Code extension (the VS Code version is preserved on the `master` branch).

## Features

Syntax highlighting within YAML block-scalars (`|` and `>`) using small
marker comments that tell the editor which language the following content
should be highlighted as.

![Example yaml file showing syntax highlighting](https://raw.githubusercontent.com/harrydowning/yaml-embedded-languages/master/images/example.png)

### Usage

To highlight a single block, place a comment with the language identifier
on the same line as the block-scalar indicator:

```yaml
example: | # python
  highlighted
```

To highlight all blocks from a point onwards, place a comment with the
extension name followed by the language identifier. In Zed the injection is
scoped to the immediately-following mapping entry (tree-sitter queries are
stateless, so the VS Code "from this point onwards" behaviour cannot be
reproduced exactly — add a marker above each block you want to highlight):

```yaml
# yaml-embedded-languages: python
example: |
  highlighted
```

### Built-in Languages

The following identifiers are accepted (see [`generate.py`](generate.py) for
the authoritative list and the Zed language each identifier maps to):

`bat`, `bibtex`, `c`, `c#`, `c++`, `clojure`, `coffee`, `cpp`, `csharp`,
`css`, `cuda`, `dart`, `diff`, `dockercompose`, `dockerfile`, `f#`,
`fsharp`, `go`, `groovy`, `handlebars`, `hlsl`, `html`, `ini`, `jade`,
`java`, `javascript`, `js`, `json`, `jsonc`, `jsonl`, `jsx`, `julia`,
`latex`, `less`, `log`, `lua`, `make`, `makefile`, `markdown`, `math`,
`objc`, `objcpp`, `perl`, `php`, `pip`, `powerfx`, `powershell`,
`properties`, `py`, `python`, `r`, `raku`, `razor`, `regex`,
`requirements`, `rst`, `ruby`, `rust`, `scss`, `shaderlab`, `shell`,
`sql`, `swift`, `tex`, `ts`, `tsx`, `typescript`, `vb`, `xml`, `xsl`,
`yaml`.

Identifiers that reference a language not present in Zed or installed via
another extension will simply not be highlighted.

## Installation

### From source (dev extension)

Zed extensions are loaded directly from a local directory while developing.
See [the Zed extension docs](https://zed.dev/docs/extensions/developing-extensions)
for the full workflow.

1. Clone this repository.
2. In Zed, open the command palette and run **`zed: extensions`**.
3. Click **`Install Dev Extension`** and select the cloned directory.
4. Open any `.yaml` / `.yml` file and add a marker comment — the embedded
   language should now be highlighted.

### Publishing

To publish this extension to the Zed extensions registry, submit a pull
request to [`zed-industries/extensions`](https://github.com/zed-industries/extensions)
adding this repository as a submodule under `extensions/yaml-embedded-languages/`.
See the Zed docs for the up-to-date procedure.

## Repository layout

```
extension.toml                 # Zed extension manifest
languages/
  yaml/
    config.toml                # YAML language config (mirrors Zed's built-in)
    injections.scm             # Tree-sitter injection queries (auto-generated)
generate.py                    # Generator for injections.scm
example.yaml                   # Sample file demonstrating all markers
```

## How it works

Zed uses tree-sitter and its injection queries to embed one language inside
another. `languages/yaml/injections.scm` contains two query patterns per
supported identifier:

1. An **inline** pattern matching a `(comment)` that immediately precedes
   a `(block_scalar)` on the same line, e.g. `key: | # python`.
2. A **block-wide** pattern matching a `(comment)` that immediately
   precedes a `(block_mapping_pair)` whose value is a block-scalar, e.g.
   `# yaml-embedded-languages: python\nkey: |\n  ...`.

Each pattern uses a `#match?` predicate on the comment text and a
`#set! injection.language` directive to select the embedded language.

## Contribution Notes

See [CONTRIBUTING](CONTRIBUTING.md).

## Release Notes

See [CHANGELOG](CHANGELOG.md).
