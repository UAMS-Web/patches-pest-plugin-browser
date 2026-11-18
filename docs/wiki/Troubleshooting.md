Written against v1.0.1.

One entry per symptom, as you meet it in a consumer: the message or behavior, its cause, and the fix.

- [The `browser` job hangs on Linux until it times out](#the-browser-job-hangs-on-linux-until-it-times-out)
- [A navigation fails with `net::ERR_NO_BUFFER_SPACE` on Windows](#a-navigation-fails-with-neterr_no_buffer_space-on-windows)
- [A slow step is retried and still times out](#a-slow-step-is-retried-and-still-times-out)
- [Composer cannot resolve `pestphp/pest-plugin-browser`](#composer-cannot-resolve-pestphppest-plugin-browser)
- [The patches are not applied](#the-patches-are-not-applied)
- [Fixed](#fixed)

## The `browser` job hangs on Linux until it times out

The browser tests finish, but the CI job does not exit, and a `node` process for `playwright run-server` is still running after the suite. It does not happen on macOS.

**Cause:** the Playwright server was started through a shell, and on Debian and Ubuntu the stop signal reaches only the shell. The server is left running and holds the job's output open.

**Fix:** confirm the [server without a shell](Patches.md#2-playwright-server-without-a-shell) patch is applied. `vendor/pestphp/pest-plugin-browser/src/Playwright/Servers/PlaywrightNpmServer.php` contains a `Temporary:` comment naming pestphp/pest#1754 when it is. If it is missing, see [The patches are not applied](#the-patches-are-not-applied).

## A navigation fails with `net::ERR_NO_BUFFER_SPACE` on Windows

A visit fails on a Windows runner, and the job log shows:

```text
pest-plugin-browser: retry 1 of 2 after <the error message> (patch from UAMS-Web/uams-statamic#585)
```

**Cause:** the operating system refused a socket. The [navigation retry](Patches.md#3-navigation-retry-on-neterr_no_buffer_space) patch retries such a navigation twice, waiting half a second and then a second, before letting the error through.

**Fix:** a retry line followed by a passing test needs nothing. When the error survives both retries, the runner is refusing sockets persistently; rerun the job, and look for something else on the runner holding connections open. Any other navigation error is not retried and is a real failure.

## A slow step is retried and still times out

An action or assertion that should finish inside the configured timeout is attempted repeatedly and fails when the timeout ends.

**Cause:** without the [per-attempt timeout](Patches.md#1-per-attempt-timeout) patch, each attempt is cut off after one second, whatever timeout is configured, so a step that needs longer never completes.

**Fix:** confirm the patch is applied: `vendor/pestphp/pest-plugin-browser/src/Execution.php` computes a `$remaining` time for each attempt when it is. If it is missing, see [The patches are not applied](#the-patches-are-not-applied).

## Composer cannot resolve `pestphp/pest-plugin-browser`

`composer require` or `composer update` refuses to install the package because of `pestphp/pest-plugin-browser`.

**Cause:** this package pins `pestphp/pest-plugin-browser` to one exact version (`5.2.0` in v1.0.1). A consumer constraint, or another package, that needs a different version cannot be satisfied at the same time. The pin is deliberate: it makes a version mismatch fail here rather than when the patches are applied.

**Fix:** loosen the consumer's own constraint on `pestphp/pest-plugin-browser` so it admits the pinned version, or wait for a release of this package that pins the version you need (see [Versioning](Versioning.md)).

## The patches are not applied

`pest-plugin-browser` behaves as it would unpatched, and the files above do not carry the patches' `Temporary:` comments.

**Cause and fix**, by the usual reasons:

- **The consumer's `vendor-dir` is not `vendor`.** Patch paths start `vendor/uams-web/patches-pest-plugin-browser/patches/`, relative to the consumer's root, so they cannot be found. This case is not supported.
- **An application required the package in `require-dev`.** `cweagans/composer-patches` applies a dependency's patches only from the `packages` section of `composer.lock`. Require it in `require`.
- **A package consumer did not list the patches.** A package that another repository installs lists the patch files in its own root `patches.json` and refreshes them with its patch commands; see [Updating a consumer](Updating-a-consumer.md).
- **`cweagans/composer-patches` is not 2.x.** The package is written for 2.x.

## Fixed

Problems fixed in a release are kept here, with the release that fixed them, instead of being deleted. None yet.
