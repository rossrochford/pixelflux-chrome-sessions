# Pixelflux fork and dependency licensing

Reviewed 2026-10-27 against the retained 2.1.0 source and the primary sources
linked below. This answers whether we can maintain and consume a fork; it does
not certify a particular future binary release's complete dependency inventory.

**Yes: pixelflux's license permits a fork containing our changes, and permits
using that fork as a dependency.** The changes can live as ordinary commits in
the fork. Applying external patch files during a build is not a license
requirement and offers no licensing exemption over committing those same changes.

## What the earlier review established

[PATCHES.md](../PATCHES.md#build-and-distribution-facts) identified MPL-2.0 for
pixelflux, the tested module's GPL feature/libx264 linkage, and bundled codec
differences. [PATCH_STRATEGY.md](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/PATCH_STRATEGY.md#distribution-until-upstream-catches-up)
already recommended consuming a pinned fork wheel, while leaving the binary
distribution decision open. That was a component inventory and proposal, rather
than a completed fork-specific license analysis.

The source identity considered here remains **2.1.0, commit
`1ebcb0a10a96caeba20e1ad09be47cc018a55238`**. No upstream version change is needed
to adopt a fork. The retained workflow source declares the default `gpl` feature,
and the validated module `215b3a04…` links libx264, as already recorded in PATCHES.md.

## MPL permissions and responsibilities

The pinned [LICENSE](https://github.com/selkies-project/pixelflux/blob/1ebcb0a10a96caeba20e1ad09be47cc018a55238/LICENSE)
is MPL-2.0. Section 2.1 grants modification, use and distribution rights, including
as part of a larger work. A separate permission from the upstream maintainers is
not required to exercise those licensed rights.

For a fork distributed outside our organization:

- Keep the MPL terms for covered source files, including our modifications to
  those files; preserve existing copyright and license notices (§§3.1, 3.4).
- When distributing a compiled extension or wheel, make its covered source
  available and tell recipients where to obtain it (§3.2). An accessible fork
  with the exact release commit and a source link accompanying the wheel is a
  practical way to supply the MPL source, provided it actually corresponds to
  the artifact and remains available to its recipients.
- Under MPL alone, separate chrome-sessions files can retain their MIT terms;
  using an MPL dependency does not require putting all our files under MPL
  (§3.3; Mozilla FAQ Q11).
- Use clear fork provenance. The license grants no general trademark rights
  (§2.3); retaining attribution does not imply upstream endorses the fork.

Mozilla confirms that private modification and use within an organization do not
trigger the external-distribution obligations (FAQ Q5–Q6). Publishing a source
fork is distribution of that source, even before publishing a wheel. There is no
requirement to contribute the modifications back upstream. See the official
[MPL text](https://www.mozilla.org/en-US/MPL/2.0/) and
[Mozilla FAQ](https://www.mozilla.org/en-US/MPL/2.0/FAQ/).

## The tested encoder build adds GPL obligations

The pinned [third-party inventory](https://github.com/selkies-project/pixelflux/blob/1ebcb0a10a96caeba20e1ad09be47cc018a55238/LICENSES.md)
states that default builds use GPL-2.0-or-later libx264 and x265, with GPL-configured
FFmpeg, and describes the resulting combined binary as distributed under GPL
terms. The validated derivative uses that default feature set and software x264.
The fork's top-level MPL declaration therefore does not describe all obligations
of its compiled artifact.

This still permits a fork and dependency use. For distribution of this build,
plan for GPL compliance for the combined runtime: applicable license notices,
corresponding source and build materials for the exact binary and its covered
components, and compatible terms for code linked with it. A public MPL fork
alone does not supply the source/notices for every bundled codec.

Our MIT source files can retain their permissive grants, but distribution of a
GPL combination must satisfy GPL terms for the combination. Packaging a library
as a separate wheel, or importing its native bindings from Python, is not by
itself a way to avoid that analysis. GNU's
[linking guidance](https://www.gnu.org/licenses/gpl-faq.html#LinkingWithGPL) and
[interpreter/bindings guidance](https://www.gnu.org/licenses/gpl-faq.html#IfInterpreterIsGPL)
describe this distinction. The extent of a combined work across the controller,
worker and subprocess boundary depends on their actual relationship; a separate
environment alone does not settle it. GNU also confirms that
[private use does not require public release](https://www.gnu.org/licenses/gpl-faq.html#GPLRequireSourcePostedPublic).

The alternative OpenH264 build is a possible packaging choice, not a condition
for forking. It must also use an appropriate FFmpeg build: disabling pixelflux's
`gpl` feature while linking a GPL system FFmpeg is insufficient to remove GPL
components. Selecting a different encoder/build would require separate runtime
validation and is outside this license review.

## Recommendation

Maintain the existing changes as commits on a fork based on the pinned 2.1.0
commit. Consume an immutable fork commit or a wheel built from that commit, with
an exact source reference and artifact hash. This makes the preferred source for
modification easy to inspect and distribute, and avoids installation-time patch
application. A Git dependency will still need the native build toolchain; a wheel
can provide the compiled dependency once its release packaging is qualified.

The license permits this maintenance approach now. Binary distribution still
needs a deliberate choice of encoder configuration and the corresponding source,
notices and build materials. This review did not create/publish a fork, change
dependencies or build configuration, upgrade pixelflux, or run display experiments.
The operator-maintained upgrade procedure remains in [UPGRADE.md](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/UPGRADE.md).

Live source snapshots used in this review remain in chrome-sessions under the ignored
`experiments/gpu-display/artifacts/license-review-20261027/.firecrawl/` directory: Mozilla's license/FAQ,
the pinned upstream license/component inventory, and GNU's GPL FAQ.
