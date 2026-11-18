Written against v1.0.1.

The package pins `pestphp/pest-plugin-browser` to exactly `5.2.0`, and every patch is written against that release. The patches are applied in this order, which is the order of `extra.patches` in `composer.json`. A later patch is cut against the output of the earlier ones, so none is reordered without re-cutting those after it.

| Order | File | Changes | Tracking ticket |
| --- | --- | --- | --- |
| 1 | `pest-plugin-browser-per-attempt-timeout.patch` | `src/Api/AwaitableWebpage.php`, `src/Execution.php` | [#2](https://github.com/UAMS-Web/patches-pest-plugin-browser/issues/2) |
| 2 | `pest-plugin-browser-server-no-shell.patch` | `src/Playwright/Servers/PlaywrightNpmServer.php` | [#3](https://github.com/UAMS-Web/patches-pest-plugin-browser/issues/3) |
| 3 | `pest-plugin-browser-navigation-no-buffer-space-retry.patch` | `src/Playwright/Page.php`, new `src/Playwright/NavigationRetry.php` | [UAMS-Web/uams-statamic#585](https://github.com/UAMS-Web/uams-statamic/issues/585) |

## 1. Per-attempt timeout

**Problem:** a retried operation or assertion ran each attempt under a fixed 1000 ms Playwright timeout, whatever timeout was configured. An operation that needed longer than a second was cut off and started again on every attempt.

**Fix:** `Execution` gives each attempt what is left of the configured timeout (at least 1 ms, at most the configured timeout), so an operation that fits the timeout finishes on its first attempt and the loop still ends when the timeout does. `AwaitableWebpage` also treats `submit()` as an action, since it is a click on the form's submit button.

**Upstream:** the same change merged in [pestphp/pest-plugin-browser#276](https://github.com/pestphp/pest-plugin-browser/pull/276), on the `5.x` line. No release carries it yet: `v5.2.0` is the newest tag and does not contain the merge commit.

**Retires when** a `pest-plugin-browser` release contains pestphp/pest-plugin-browser#276.

## 2. Playwright server without a shell

**Problem:** the plugin started `playwright run-server` through a shell. Where `/bin/sh` is dash (Debian and Ubuntu, so a Linux CI runner), the stop signal reached only the shell, and the Node server was left running with nobody to stop it. With output piped, as a CI runner always does, the leftover process held the output open and the `browser` job hung until its timeout. On macOS the shell replaces itself with the server, so the problem does not show there.

**Fix:** the server is started from an argument list instead of a command line, so on Unix no shell sits between the plugin and the server and the stop signal reaches it. On Windows the command is built as before, so nothing changes there.

**Upstream:** reported as [pestphp/pest#1754](https://github.com/pestphp/pest/issues/1754), which is open.

**Retires when** a `pest-plugin-browser` release stops the server being left running. When pestphp/pest#1754 closes, check whether its fix removes the need for this patch.

## 3. Navigation retry on `net::ERR_NO_BUFFER_SPACE`

**Problem:** on a hosted Windows runner, a navigation to a local test page once failed with Chromium's `net::ERR_NO_BUFFER_SPACE`, because the operating system refused a socket, while the same code passed on another attempt. The failure comes from the runner, not the page.

**Fix:** `Page::goto()` runs through `NavigationRetry::onNoBufferSpace()`, which retries a navigation that failed with that error only:

| Constant | Value | Meaning |
| --- | --- | --- |
| `NavigationRetry::ERROR` | `net::ERR_NO_BUFFER_SPACE` | The one error that is retried. Any other navigation error is thrown at once. |
| `NavigationRetry::RETRIES` | `2` | Retries before the error is thrown. |
| `NavigationRetry::BACKOFF_MICROSECONDS` | `500000` | Wait before the first retry; each later retry waits one step longer. |

Each retry writes one line to standard error, so a job log shows it:

```text
pest-plugin-browser: retry 1 of 2 after <the error message> (patch from UAMS-Web/uams-statamic#585)
```

**Upstream:** no upstream report is tracked; the ticket that added it is [UAMS-Web/uams-statamic#585](https://github.com/UAMS-Web/uams-statamic/issues/585).

## Reference

- [Verifying the patch set](Verifying-the-patch-set.md)
- [Upstream watches](Upstream-watches.md)
- [Troubleshooting](Troubleshooting.md)
