Written against v1.0.1.

The package carries its own semantic version, and each release pins exactly one `pestphp/pest-plugin-browser` version.

| Change | Release |
| --- | --- |
| A patch's content changes, same upstream pin | patch (`1.0.1`) |
| A patch is added or retired, or the pin moves within the upstream major | minor (`1.1.0`) |
| The pin moves to a new upstream major | major (`2.0.0`) |

A consumer requires the package with a caret range (`^1.0`), so it takes patch and minor releases and stays on the current upstream major. Update with `--with-dependencies` so the upstream package moves with the pin; [Updating a consumer](Updating-a-consumer.md) says why.

## Example

`v1.0.1` changed the content of the per-attempt timeout patch to match pestphp/pest-plugin-browser#276 and kept the `5.2.0` pin, so it was a patch release over `v1.0.0`.

## Releases

- [Releases](https://github.com/UAMS-Web/patches-pest-plugin-browser/releases)
- [Patches](Patches.md): what the current release carries.
