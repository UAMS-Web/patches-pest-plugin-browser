Written against v1.0.1.

A consumer already requires this package and a new release is out. Move it so the patches and the upstream package move together.

1. **Update this package with its dependencies:**

   ```bash
   composer update uams-web/patches-pest-plugin-browser --with-dependencies
   ```

   Each release pins exactly one `pestphp/pest-plugin-browser` version, so the upstream package has to be free to move with it. Without `--with-dependencies`, Composer holds `pestphp/pest-plugin-browser` at its locked version and quietly stays on the newest release of this package that pins that version.

2. **Read the lock back.** In `composer.lock`, `uams-web/patches-pest-plugin-browser` shows the release you meant to install, and `pestphp/pest-plugin-browser` shows the version that release pins (see [Patches](Patches.md)). A matching exit status is not enough: Composer reports success whichever release it chose.

3. **Confirm the patches were applied.** A package that another repository installs lists the patch files in its own root `patches.json`; refresh those with the consumer's own patch commands (`composer patches-relock`, then `composer patches-repatch`), as the consumer's `upstream-patch-lifecycle` skill describes. An application picks the patches up from this package's `extra.patches` and needs no list of its own.

4. **Run the consumer's browser suite** on the platforms it supports. The patches change behavior only where `pest-plugin-browser` runs.

**Note:** patch paths start `vendor/uams-web/patches-pest-plugin-browser/patches/`, relative to the consumer's root. A consumer with a non-default `vendor-dir` is not covered.

## Reference

- [Installation (README)](https://github.com/UAMS-Web/patches-pest-plugin-browser#install)
- [Versioning](Versioning.md)
- [Troubleshooting](Troubleshooting.md)
