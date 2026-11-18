Written against v1.0.1.

An upstream release now carries the fix a patch exists for. The patch retires from this package once, rather than from each consumer.

1. **Confirm the fix shipped in a tag.** Check the tags, not the GitHub Releases feed, and check that the tag contains the fix rather than only that it is newer:

   ```bash
   gh api 'repos/pestphp/pest-plugin-browser/tags?per_page=5' --jq '.[].name'
   gh api repos/pestphp/pest-plugin-browser/compare/<tag>...<merge-commit-sha> --jq '.status'
   ```

   `behind` or `identical` means the tag contains the merge commit. `ahead` means it does not, and the fix is still unreleased. The [Upstream watches](Upstream-watches.md) workflow runs the same check daily and comments on the tracking ticket when it changes.

2. **Move the pin and drop the patch in one release.** In `composer.json`, set `require` to the upstream version that carries the fix, remove the patch's `extra.patches` entry, and delete its file under `patches/`. Re-cut any later patch against the new upstream version, then verify the remaining set against the new tag as [Verifying the patch set](Verifying-the-patch-set.md) describes.

3. **Retire the watch:** delete the patch's row under `.github/upstream-watches/` and its note in that folder's README, and close the tracking ticket.

4. **Update the [Patches](Patches.md) page** and release. Moving the pin within the upstream major and retiring a patch are both a minor release (see [Versioning](Versioning.md)).

5. **Consumers adopt the release** as [Updating a consumer](Updating-a-consumer.md) describes. A consumer that listed the retired patch in its own `patches.json` removes that entry.

When the last patch retires, the repository is archived, and each consumer removes the package.

## Reference

- [Patches](Patches.md)
- [Upstream watches](Upstream-watches.md)
- [Versioning](Versioning.md)
