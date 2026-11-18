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

## Documentation

The [wiki](https://github.com/UAMS-Web/patches-pest-plugin-browser/wiki) documents the package: each patch, what it changes and when it retires ([Patches](https://github.com/UAMS-Web/patches-pest-plugin-browser/wiki/Patches)), how releases are versioned ([Versioning](https://github.com/UAMS-Web/patches-pest-plugin-browser/wiki/Versioning)), how to verify the set against the pinned release ([Verifying the patch set](https://github.com/UAMS-Web/patches-pest-plugin-browser/wiki/Verifying-the-patch-set)), and troubleshooting. Its pages are kept in [`docs/wiki/`](docs/wiki/) and published from there.

## License

MIT, see [`LICENSE`](LICENSE). The patches modify `pestphp/pest-plugin-browser`, which is also MIT licensed.
