# uams-web/patches-pest-plugin-browser

Patches that UAMS-Web repositories apply to [`pestphp/pest-plugin-browser`](https://github.com/pestphp/pest-plugin-browser) with [`cweagans/composer-patches`](https://github.com/cweagans/composer-patches) 2.x. One package replaces the copies each repository carried, so a patch is changed once and retires everywhere in one release.

This package pins `pestphp/pest-plugin-browser` to exactly `5.2.0`. Every patch is written against that release.

## Install

This package is read from GitHub with a `vcs` repository, not from Packagist. The consumer's root `composer.json` lists it (Composer reads `repositories` only from the root project):

```json
"repositories": [
  { "type": "vcs", "url": "https://github.com/UAMS-Web/patches-pest-plugin-browser.git" }
]
```

An application requires it in `require` (the plugin applies a dependency's patches only from `packages` in `composer.lock`):

```bash
composer require --no-plugins uams-web/patches-pest-plugin-browser:^1.0
```

A package that another repository installs requires it with `--dev` and lists the patch files in its own root `patches.json`. The full procedure, including `composer patches-relock` and `composer patches-repatch`, is the `upstream-patch-lifecycle` skill's *Moving a consumer to the package*.

Patch paths are relative to the consumer's root and start `vendor/uams-web/patches-pest-plugin-browser/patches/`, so a consumer with a non-default `vendor-dir` is not covered.

## Patches

Applied in this order, which is the order in `composer.json`. A later patch is cut against the output of the earlier ones, so none is reordered without re-cutting those after it.

- **`pest-plugin-browser-per-attempt-timeout.patch`**: Give each retry attempt the configured Playwright timeout instead of a hardcoded 1000ms (temporary; see UAMS-Web/uams-statamic-archive#1816)
- **`pest-plugin-browser-server-no-shell.patch`**: Start the Playwright server without a shell so Linux cannot orphan it (temporary; see UAMS-Web/uams-statamic-archive#1818)
- **`pest-plugin-browser-navigation-no-buffer-space-retry.patch`**: Retry a navigation the Windows runner refuses with net::ERR_NO_BUFFER_SPACE (temporary; see UAMS-Web/uams-statamic#585)

Each description names the ticket that tracks the upstream fix, where there is one.

## Versioning

| Change | Release |
| --- | --- |
| A patch's content changes, same upstream pin | patch |
| A patch is added or retired, or the pin moves within the upstream major | minor |
| The pin moves to a new upstream major | major |

## Verifying the set

Apply the 3 patches in order to the `v5.2.0` tag of `pestphp/pest-plugin-browser` with `git apply -p1`. Applying the first patch a second time must fail.

## License

MIT, see [`LICENSE`](LICENSE). The patches modify `pestphp/pest-plugin-browser`, which is also MIT licensed.
