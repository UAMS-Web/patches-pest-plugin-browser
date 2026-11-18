Written against v1.0.1.

At the end of this walkthrough you will have applied every patch in the package, in order, to the upstream release it pins, and shown that the set is neither missing a hunk nor already applied. Run it after any change to a patch, before releasing.

1. **Clone the pinned upstream release.** The package pins `pestphp/pest-plugin-browser` to exactly `5.2.0` in `composer.json`, so clone that tag:

   ```bash
   git clone --depth 1 --branch v5.2.0 https://github.com/pestphp/pest-plugin-browser.git upstream
   cd upstream
   ```

2. **Apply the patches in the order `composer.json` lists them.** A later patch is cut against the output of the earlier ones, so the order matters. With this repository checked out beside `upstream` as `patches-pest-plugin-browser`:

   ```bash
   P=../patches-pest-plugin-browser/patches
   git apply -p1 "$P/pest-plugin-browser-per-attempt-timeout.patch"
   git apply -p1 "$P/pest-plugin-browser-server-no-shell.patch"
   git apply -p1 "$P/pest-plugin-browser-navigation-no-buffer-space-retry.patch"
   ```

   Each command prints nothing and exits `0`. `git status --short` then shows four modified files under `src/` and one new file, `src/Playwright/NavigationRetry.php`.

3. **Apply the first patch a second time.** It must fail:

   ```bash
   git apply -p1 "$P/pest-plugin-browser-per-attempt-timeout.patch"
   ```

   ```text
   error: patch failed: src/Api/AwaitableWebpage.php:42
   error: src/Api/AwaitableWebpage.php: patch does not apply
   ```

   A patch that applies twice is not changing what it claims to change.

**Note:** this checks that the patches apply. It does not run `pest-plugin-browser`'s tests or a consumer's browser suite; those run in the consumers.

## Going further

- [Changing a patch](Changing-a-patch.md): what to re-cut when a patch changes.
- [Patches](Patches.md): what each patch changes and why.
