Written against v1.0.1.

The `Watch upstream fixes` workflow (`.github/workflows/watch-upstream.yml`) checks, once a day, the upstream issues and pull requests this package is waiting on. It comments on the tracking ticket in this repository only when something moves: an issue closes, a pull request merges or closes, or a release ships the merge. A weekly heartbeat comment on each open ticket shows the watch is still running. Closing a tracking ticket turns its watch off.

## Rows

Each watch is one JSON file in `.github/upstream-watches/`, named `<tracker>-<owner>-<repo>-<number>.json`. That folder's [README](https://github.com/UAMS-Web/patches-pest-plugin-browser/blob/main/.github/upstream-watches/README.md) documents every key.

| Key | Required | Meaning |
| --- | --- | --- |
| `tracker` | yes | The issue in this repository that receives the comments. |
| `repo` | yes | The upstream repository, `owner/name`. |
| `kind` | yes | `issue` or `pr`. |
| `number` | yes | The upstream issue or pull request number. |
| `composer` | no | A package name, to report a Packagist release newer than `composer.lock`. |
| `released` | no | On a `pr` row, report the first release on the pull request's base branch line that contains the merge. |

## Current rows

| File | Watches | Tracking ticket | Patch |
| --- | --- | --- | --- |
| `2-pestphp-pest-plugin-browser-276.json` | [pestphp/pest-plugin-browser#276](https://github.com/pestphp/pest-plugin-browser/pull/276), `released: true` | [#2](https://github.com/UAMS-Web/patches-pest-plugin-browser/issues/2) | Per-attempt timeout |
| `3-pestphp-pest-1754.json` | [pestphp/pest#1754](https://github.com/pestphp/pest/issues/1754) | [#3](https://github.com/UAMS-Web/patches-pest-plugin-browser/issues/3) | Playwright server without a shell |

No row carries `composer`. The workflow reads the installed version from `composer.lock`, and this package commits none: it is a library that pins `pestphp/pest-plugin-browser` in `composer.json`.

## Adding and retiring a row

- **Add** a row in the pull request that adds the patch it exists for.
- **Retire** a row in the pull request that drops the patch: delete the file and close the tracking ticket.
- Give two upstream threads separate tracking tickets whenever either can outlive the other, since closing a ticket stops every row under it.

## Running it by hand

The workflow also runs from the Actions tab (*Watch upstream fixes*, *Run workflow*), which is how to test a new row without waiting a day.

## Reference

- [Retiring a patch](Retiring-a-patch.md)
- [Patches](Patches.md)
