`uams-web/patches-pest-plugin-browser` carries the patches that UAMS-Web repositories apply to [`pestphp/pest-plugin-browser`](https://github.com/pestphp/pest-plugin-browser) through [`cweagans/composer-patches`](https://github.com/cweagans/composer-patches) 2.x. A consumer requires this one package instead of keeping its own copy of each patch, so a patch changes once and retires everywhere in one release.

This wiki holds the documentation: a walkthrough, task guides, troubleshooting and the reference. The [README](https://github.com/UAMS-Web/patches-pest-plugin-browser#readme) keeps the overview and installation.

## Getting started

- [Installation (README)](https://github.com/UAMS-Web/patches-pest-plugin-browser#install): add the package to a consumer's `composer.json`.
- [Verifying the patch set](Verifying-the-patch-set.md): apply every patch to the pinned upstream release by hand and confirm the set is sound.

## Guides

- [Updating a consumer](Updating-a-consumer.md): move a consumer to a new release of this package.
- [Changing a patch](Changing-a-patch.md): edit, add or reorder a patch and release the change.
- [Retiring a patch](Retiring-a-patch.md): drop a patch once an upstream release carries its fix.

## Troubleshooting

- [Troubleshooting](Troubleshooting.md): one entry per symptom, with its cause and fix.

## Reference

- [Patches](Patches.md): each patch, the files it changes, its tracking ticket and when it retires.
- [Versioning](Versioning.md): which change makes a patch, minor or major release.
- [Upstream watches](Upstream-watches.md): the daily workflow that reports when an upstream fix moves.

## Project

- [Project](Project.md): where the package is developed, how to report a problem, and how these pages are published.
