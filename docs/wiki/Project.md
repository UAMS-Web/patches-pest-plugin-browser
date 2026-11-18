Written against v1.0.1.

## Where it is developed

The package is developed in [UAMS-Web/patches-pest-plugin-browser](https://github.com/UAMS-Web/patches-pest-plugin-browser) and read by consumers through a `vcs` repository entry, not from Packagist. Changes land through pull requests to `main`; [CONTRIBUTING.md](https://github.com/UAMS-Web/patches-pest-plugin-browser/blob/main/CONTRIBUTING.md) describes the path from issue to merge.

## Reporting a problem

Open an issue in [this repository](https://github.com/UAMS-Web/patches-pest-plugin-browser/issues) for a patch that fails to apply, a wrong pin, or a problem these pages describe incorrectly. A defect in `pest-plugin-browser` itself belongs upstream, in [pestphp/pest-plugin-browser](https://github.com/pestphp/pest-plugin-browser) (or [pestphp/pest](https://github.com/pestphp/pest) where that project tracks it); the patch here is the stopgap until a release carries the fix.

## Releases

Each release is a tag on `main` with notes on the [Releases](https://github.com/UAMS-Web/patches-pest-plugin-browser/releases) page. [Versioning](Versioning.md) gives which change makes which kind of release.

## These pages

These pages live in [`docs/wiki/`](https://github.com/UAMS-Web/patches-pest-plugin-browser/tree/main/docs/wiki) and are reviewed like any other change. The `Publish wiki` workflow (`.github/workflows/publish-wiki.yml`) publishes the folder to the wiki when a release is published, and it can be run by hand from the Actions tab. Each publish replaces the whole wiki with the folder, so a page edited in the browser is overwritten: edit the folder through a pull request instead.

## License

MIT, see [LICENSE](https://github.com/UAMS-Web/patches-pest-plugin-browser/blob/main/LICENSE). The patches modify `pestphp/pest-plugin-browser`, which is also MIT licensed.
