# Contributing

## Prerequisites

- Python 3.8+ (only required to regenerate `languages/yaml/injections.scm`)
- [Zed](https://zed.dev) for manual testing

## Adding or changing a supported language

The list of accepted identifiers and their target Zed languages lives in the
`LANGUAGES` table at the top of [`generate.py`](generate.py). After editing
it, regenerate the injection queries and commit both files:

```sh
python3 generate.py
```

`generate.py` performs a basic sanity check (balanced parentheses, trailing
`)`) on the generated file and will exit non-zero if something is wrong.

## Testing locally

1. Open Zed and run the **`zed: extensions`** command.
2. Click **`Install Dev Extension`** and point it at this repository.
3. Open [`example.yaml`](example.yaml) and verify that each marker produces
   the expected embedded-language highlighting.
4. When iterating, re-run `Install Dev Extension` (or Zed's dev-extension
   reload affordance) after each change.

## Checks run in CI

- `python3 generate.py` must leave the working tree clean (i.e. the
  committed `languages/yaml/injections.scm` must match the output of the
  generator).
- The generator's own sanity check must pass.
