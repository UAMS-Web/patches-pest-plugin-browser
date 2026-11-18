Written against v1.0.1.

A patch needs to change, a new one needs adding, or the set needs reordering. Every patch here is hand-written: edit the `.patch` file directly. There is no generator.

1. **Branch from `main`** and edit the file under `patches/`. Keep the comment the patch adds to the upstream source (each one opens with `Temporary:` and names its ticket), so a reader of the patched file can find out why it is there.

2. **Keep `composer.json` in step.** `extra.patches` lists each patch with a description and its path, starting `vendor/uams-web/patches-pest-plugin-browser/patches/`. The description says what the patch fixes, that it is temporary, and the ticket that tracks the upstream fix. A new patch adds an entry; a renamed file changes its path.

3. **Re-cut every patch after the one you changed.** The listed order is the order the patches are applied in, and a later patch is cut against the output of the earlier ones. Changing or reordering one can stop those after it from applying, so re-cut them against the new output rather than editing their line numbers by hand.

4. **Verify the set** as [Verifying the patch set](Verifying-the-patch-set.md) describes: every patch applies in order to the pinned tag, and the first fails a second time.

5. **Update the documentation in the same pull request:** the [Patches](Patches.md) page, and an upstream watch row for a new patch whose fix is tracked upstream (see [Upstream watches](Upstream-watches.md)).

6. **Release it** with the version change [Versioning](Versioning.md) gives: a patch release for changed content under the same pin, a minor release for an added patch.

## Reference

- [Patches](Patches.md)
- [Versioning](Versioning.md)
- [Upstream watches](Upstream-watches.md)
