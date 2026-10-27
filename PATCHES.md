# chrome-sessions fork of pixelflux 2.1.0

The maintained source is on `chrome-sessions-2.1.0`, based on upstream
`1ebcb0a10a96caeba20e1ad09be47cc018a55238` (tag `2.1.0`). The five existing
patches were imported unchanged on 2026-10-27 as these commits:

| Original patch | Fork commit |
|---|---|
| `pixelflux-windowed.patch` | [efc1c82](https://github.com/rossrochford/pixelflux-chrome-sessions/commit/efc1c823a05b3defe102e2692653d6eda5260665) |
| `pixelflux-audio-anchor.patch` | [0eb3265](https://github.com/rossrochford/pixelflux-chrome-sessions/commit/0eb3265b70a41088b94ddb8e3e4f420f740ea79d) |
| `pixelflux-refresh-59963.patch` | [a6226fa](https://github.com/rossrochford/pixelflux-chrome-sessions/commit/a6226fa7152a691421f95fe1dacfce872fbb81e3) |
| `pixelflux-capture-cadence.patch` | [1ca239b](https://github.com/rossrochford/pixelflux-chrome-sessions/commit/1ca239b464e486537ec05b9156ad901ca045de90) |
| `pixelflux-window-origin.patch` | [ddb6b54](https://github.com/rossrochford/pixelflux-chrome-sessions/commit/ddb6b548ca5d66e7415038173a500861013f78cf) |

Before documentation changes, all 93 original files matched the retained
validated source tree byte-for-byte, including `pixelflux/Cargo.lock`. Only
`pixelflux/src/lib.rs`, `pixelflux/src/recorder/mod.rs` and
`pixelflux/src/wayland/frontend.rs` differ from upstream source. The two existing
audio guard tests are preserved. See [migration validation](docs/FORK_MIGRATION.md)
for fresh native test results and their limits.

The evidence below is the historical 2026-10-21 inventory. Its release/head
comparisons and proposed settings are dated analysis, not a version upgrade or
new fork API. Relative paths in code spans (including `artifacts/`) refer to
chrome-sessions' `experiments/gpu-display/` unless identified as pixelflux source;
ignored raw artifacts remain local to the experiment host. Patch file links now
lead to the corresponding fork commits.

The latest retained host evidence is [aligned visible/private controls](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/hosts/intel-irisxe-ubuntu2604/ALIGNED_CONTROLS_20261021.md)
and [aligned workflow validation](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/hosts/intel-irisxe-ubuntu2604/ALIGNED_WORKFLOW_20261021.md).
These cover the identical source with the retained module on Ubuntu/Intel; they
do not qualify a newly built wheel or other hosts. Experiment policy and evidence
remain in chrome-sessions: [brief](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/BRIEF.md),
[patch strategy](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/PATCH_STRATEGY.md),
[upgrade procedure](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/UPGRADE.md), and
[current progress](https://github.com/rossrochford/chrome-sessions/blob/main/experiments/gpu-display/PROGRESS.md).
The [fork license review](docs/FORK_LICENSE.md) covers MPL source obligations and
the tested GPL codec build; binary release packaging remains a separate decision.

## Historical patch inventory (2026-10-21)

| Patch | Part | Lines added / removed | Bug or mode-specific need | Applies alone / stacked to latest release | Applies alone / stacked to head | Related upstream link |
|---|---|---:|---|---|---|---|
| `pixelflux-windowed.patch` | Fixed normal-window policy and client-side decorations | +35 / −17 | Mode-specific; fullscreen is intentional upstream policy | Yes / yes | Yes / yes | [PR #23](https://github.com/selkies-project/pixelflux/pull/23), historical architecture, not this option |
| `pixelflux-audio-anchor.patch` | Start audio reader before capture setup; guard early-start failure; two guard tests | +83 / −13 | Timestamp bug with a live audio source and delayed setup; guard supports the fix | No / no | No / no | [3b549ba](https://github.com/selkies-project/pixelflux/commit/3b549ba29d59be4c87beae92ef8714b11639b78b), a different audio-timestamp fix |
| `pixelflux-refresh-59963.patch` | Advertise 59.963 Hz instead of 60 Hz, including resize fallback | +4 / −4, excluding shared constant | Host-specific display configuration | No / no | No / no | [Base output implementation](https://github.com/selkies-project/pixelflux/blob/1ebcb0a10a96caeba20e1ad09be47cc018a55238/pixelflux/src/lib.rs) |
| Same | Replace 250 ms idle callbacks with approximately 16.677 ms callbacks; introduce shared rate constant | +10 / −24 | Mode-specific continuous background animation requirement; upstream deliberately throttles idle clients | No / no | No / no | [Selkies #402](https://github.com/selkies-project/selkies/issues/402), related input/capture scheduling, not this requirement |
| Same | Set initial backend `settings.target_fps` to 59.963 | +1 / −0 | Mode-specific initial/backend pacing, not a general encoder-rate fix | No / no | No / no | [31753b7](https://github.com/selkies-project/pixelflux/commit/31753b73fb5c556d30e47888ab572bb1007c8118), distinct fractional encoder-rate fix |
| Same | Change no-capture presentation-refresh fallback from unknown to fixed | +1 / −1 | Presentation-policy choice; subsequently removed by the capture-cadence patch | No / no | No / no | [Base presentation implementation](https://github.com/selkies-project/pixelflux/blob/1ebcb0a10a96caeba20e1ad09be47cc018a55238/pixelflux/src/lib.rs) |
| `pixelflux-capture-cadence.patch` | Independent client-callback timer; remove render/idle/input-wake callback scheduling | +12 / −52 for whole patch, shared with next row | Mode-specific separation of application cadence from recording/viewer rate | No / no | No / no | [Selkies #402](https://github.com/selkies-project/selkies/issues/402), related but different scheduling goal |
| Same | Discard presentation feedback even during capture instead of reporting capture as presentation | Included above | Offscreen presentation-policy choice; a general correctness claim is not established | No / no | No / no | [Base presentation implementation](https://github.com/selkies-project/pixelflux/blob/1ebcb0a10a96caeba20e1ad09be47cc018a55238/pixelflux/src/lib.rs) |
| `pixelflux-window-origin.patch` | Use surface origin, rather than window-geometry origin, in both renderers | +2 / −2 | Coordinate bug for directly composited surfaces with nonzero geometry offsets | No / no | No / no | [d6a1939](https://github.com/selkies-project/pixelflux/commit/d6a1939b2bdf09113a90ef706c444c5b4ba299f4), related popup fix, not this root-surface fix |

Application columns test the **whole patch file**, not hypothetical separated parts. The refresh file totals **+16 / −29**; its four rows partition its hunks, with the rate constant counted with idle pacing. Capture-cadence hunks mix callback and presentation changes, so its total is stated once rather than assigning misleading independent counts. “Stacked” means checked after preceding successfully applied patches in build order; failures were skipped and the remaining patches still checked. Latest release and head currently name the same commit, but both sets of checks were run.

# Pixelflux derivative: patch facts, 2026-10-21

This is evidence for a later per-patch upstream/fork decision, not that decision. **Verified** below means inspected source, metadata, archive/binary bytes or an actual application check in this session. Runtime results are explicitly attributed to existing experiment reports. **Inferred** assessments describe other topologies and proposed configuration surfaces; no new runtime, compile or performance test was run.

Host: `ross-ThinkPad-X1-Carbon-Gen-10`, Ubuntu 26.04.1 LTS, Intel laptop. All repository access was read-only except this document and one appended progress entry. Scratch clones, extracted copies and check logs are under `/tmp/pixelflux-patchcheck/`. No Cargo build/test, Chrome launch or compositor start was performed.

## Verified base, order and upstream snapshot

**The supposed PyPI sdist is actually a GitHub tag archive.** The retained `artifacts/upstream/pixelflux-2.1.0.tar.gz` has SHA-256:

```text
142f42f5ad99e61b128c2907e8f7bb6ea743d1283d6d17a9c7e833b34c46c973
```

`build_windowed.py:main()` downloads [the GitHub 2.1.0 tag archive](https://codeload.github.com/selkies-project/pixelflux/tar.gz/refs/tags/2.1.0), not a PyPI source distribution. The [PyPI 2.1.0 JSON listing](https://pypi.org/pypi/pixelflux/2.1.0/json), checked in this session, contains 28 files, all `bdist_wheel`, and no sdist. The older [window-policy report](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/WINDOW_POLICY_RESULTS.md#window-policy-and-source-build) records the correct archive hash but does not call it a PyPI sdist.

The archive maps to tag `2.1.0`, commit **`1ebcb0a10a96caeba20e1ad09be47cc018a55238`**. A fresh extraction and `git archive 2.1.0` have exactly the same **93 file paths and file contents**, with no additions, deletions or changed bytes. The crate lives under `pixelflux/`, not at repository root. Committed `Cargo.lock`, the `nvcodec-sys` crate, CUDA/NVENC binding files, NVIDIA headers and the KDE protocol XMLs are present and identical. There is no sdist-specific layout or generated/vendor-file delta to compensate for. Build-generated files live under `target/`, which was excluded from the built-tree comparison. The built source directory has no `.git` directory.

**Build order**, verified in `build_windowed.py:main()` and the insertion order of `artifacts/windowed-build/build-module-workflow.json:extra_patches`:

1. `pixelflux-windowed.patch`
2. `pixelflux-audio-anchor.patch`
3. `pixelflux-refresh-59963.patch`
4. `pixelflux-capture-cadence.patch`
5. `pixelflux-window-origin.patch`

The helper uses `patch -p1` with the extracted source root as working directory, applying the window patch first, then `--extra-patch` arguments in command-line order. **Qualification:** it applies patches only when creating a source directory; if that directory already exists, it reuses it without reapplying/revalidating the stack. It is not an unconditional clean-build recipe.

All five current patch hashes match the recorded build, individually listed below. Applying them in that order to the newly extracted archive reconstructed **all 93 non-`target/` files byte-for-byte** in `artifacts/windowed-build/pixelflux-2.1.0-module-workflow/`. Relative to the clean archive, only these three source files change:

```text
pixelflux/src/lib.rs
pixelflux/src/recorder/mod.rs
pixelflux/src/wayland/frontend.rs
```

**No unaccounted change was found.** The older `artifacts/windowed-build/pixelflux-2.1.0/` separately matches the fresh archive plus the window patch alone; it is indeed not pristine. The built module still hashes to **`215b3a04a4f1d9ae1a0f839d314221101fc8f7dfc93199452fa58a8765e8e8ab`**, matching the build record. The built tree's `Cargo.lock` matches its recorded hash **`d3023efa7a99e21411b35026d03bc7dbadaab3f0ebe940a4110c691bd210ef76`**.

**Upstream checked on 2026-10-21 UTC:** [selkies-project/pixelflux](https://github.com/selkies-project/pixelflux), default branch `main`. [Latest stable release 2.2.1](https://github.com/selkies-project/pixelflux/releases/tag/2.2.1), published `2026-10-21T03:27:43Z`, and fetched `origin/main` both resolve to **`67b6d833fc8eba87f9b15fb4cb3382d923a2fe56`**. The release API's latest stable result was distinguished from the per-commit prereleases. Selkies topology source was checked at **`bcb5d67aa4c1182b48bb9a562037b8bc9092b447`**.

### Application-check method and full results

Cloned with `git clone --no-checkout https://github.com/selkies-project/pixelflux.git /tmp/pixelflux-patchcheck`. Exported separate clean trees using `git archive` for the tag, release and head; the base check uses the freshly extracted retained archive. Each solo check used its own untouched copy. Each cumulative run used a separate copy, applying a patch with `git apply` only after its check succeeded. A failed patch was left unapplied; its dry run did not partially modify the cumulative tree.

Commands, from each exported/extracted source root:

```bash
GIT_CEILING_DIRECTORIES=/tmp/pixelflux-patchcheck/analysis \
  git apply --check -p1 /absolute/path/to/the.patch
# On successful cumulative checks:
GIT_CEILING_DIRECTORIES=/tmp/pixelflux-patchcheck/analysis \
  git apply -p1 /absolute/path/to/the.patch
# On every failed strict check, using the patch as stdin:
patch --batch --forward -p1 --dry-run < /absolute/path/to/the.patch
```

The ceiling prevents exported copies inside the scratch clone from inheriting the parent repository's path prefix. **No `--directory` adjustment was required:** base, release and head all retain `pixelflux/src/...`; stripping `a/` or `b/` with `-p1` is correct. Success means applicable context, possibly relocated; it does not mean compiled or runtime-validated. No edited patch, automatic rejection repair or fuzzy application was used to make a strict check pass.

| Patch | 2.1.0 alone | 2.1.0 cumulative | 2.2.1 alone | 2.2.1 cumulative | Head alone | Head cumulative |
|---|---|---|---|---|---|---|
| Window policy | Pass | Pass | Pass | Pass | Pass | Pass |
| Audio anchor | Pass | Pass | Fail | Fail, skipped | Fail | Fail, skipped |
| Refresh / idle | Pass | Pass | Fail | Fail, skipped | Fail | Fail, skipped |
| Capture cadence | Fail | Pass | Fail | Fail, skipped | Fail | Fail, skipped |
| Render origin | Pass | Pass | Fail | Fail, skipped | Fail | Fail, skipped |

All failed `patch --dry-run` checks also return nonzero. Complete diagnostics are retained outside the repository in `analysis/patch-matrix.json`. Their hunk-level results are recorded here so the facts do not depend on that temporary file surviving:

| Target / patch | First conflicting original hunk | Dry-run failures, offsets and fuzz |
|---|---|---|
| 2.1.0 solo capture cadence | `lib.rs`, hunk 1, `@@ -3421,38 +3421,8 @@` | H1 fails at 3421; H2 succeeds at 4792 (+3); H3 at 5433 (+3); H4 at 5494 (+3); H5 fails at 5569. No fuzz reported. |
| Latest and head, audio, solo **and** cumulative | `recorder/mod.rs`, hunk 3, `@@ -673,19 +713,7 @@` | H1 succeeds at 591 (+76); H2 at 669 (+79); H3 fails at 713; H4 succeeds at 1142 with **fuzz 2**, offset +220. Existing tests and reformatted thread-spawn code also need consideration during any manual port. |
| Latest and head, refresh, solo | `lib.rs`, hunk 2, `@@ -3449,7 +3448,7 @@` | H1 succeeds at 1368 (+248); H2 and H3 fail at 3448 and 3807; H4 succeeds at 4928 (+780); H5 at 5455 (+813); H6 at 5525 (+816); H7 at 5533 (+816); H8 fails at 5575. No fuzz reported. |
| Latest and head, refresh, cumulative | Same hunk 2 | Same failures and H1 offset; H4 succeeds at 4925 (+777), H5 at 5452 (+810), H6 at 5522 (+813), H7 at 5530 (+813). The earlier window patch accounts for the three-line difference from solo. |
| Latest and head, capture cadence, solo | `lib.rs`, hunk 1, `@@ -3421,38 +3421,8 @@` | H1 fails at 3421; H2 succeeds at 5614 (+825); H3 at 6420 (+990); H4 at 6485 (+994); H5 fails at 5569. No fuzz reported. |
| Latest and head, capture cadence, cumulative | Same hunk 1 | H1/H5 still fail; H2 succeeds at 5611 (+822), H3 at 6417 (+987), H4 at 6482 (+991). Refresh was skipped, so this is not a complete ported dependency stack. |
| Latest and head, render origin, solo **and** cumulative | `lib.rs`, hunk 1, `@@ -3181,7 +3181,7 @@` | Both H1 and H2 fail at 3181 and 3345. No successful hunks, offsets or fuzz reported. |

Latest/head diagnostics were identical for each corresponding run. A separate dependency check applied only refresh to clean 2.1.0: capture cadence then passed `git apply --check`, proving its textual dependency does not require the window or audio patches.

Other post-base changes affecting these code paths include [ffe1b8d](https://github.com/selkies-project/pixelflux/commit/ffe1b8d70375498b41051825d3df3896255f7e57), which reformats the Rust source; [a541a5c](https://github.com/selkies-project/pixelflux/commit/a541a5c993995244fcd65baff36bd662db523cd9), which changes GLES readback around the render paths; [5e71ebd](https://github.com/selkies-project/pixelflux/commit/5e71ebdad928c16f0ec1fc89e52d324ffd237723), which updates Smithay for late pointer entry; [e430138](https://github.com/selkies-project/pixelflux/commit/e4301382a3288327cd16db520a64773089249764), which changes pointer focus on mapping; and [c1730eb](https://github.com/selkies-project/pixelflux/commit/c1730eb4c5fa1b789f488d92f8b32e6fd23841ea), which changes render/encoder paths for 10-bit capture and several backend fixes. These are related source movement/behavior changes, not evidence that the five local requirements are implemented. The audio, popup and pacing changes most directly relevant to each patch are discussed below; [the full base-to-release comparison](https://github.com/selkies-project/pixelflux/compare/2.1.0...2.2.1) supplies the broader history.

### Topologies and configuration surfaces used in the assessments

The following are verified distinctions, not assumptions that every deployment runs Chrome directly on pixelflux:

- **Nested:** [Selkies `input_handler.py`](https://github.com/selkies-project/selkies/blob/bcb5d67aa4c1182b48bb9a562037b8bc9092b447/src/selkies/input_handler.py#L6794) resolves `_app_wayland_display()` separately from the capture compositor. `_adopt_app_wl_display()` calls `set_app_wayland_display`; `SESSION_IPC_SOCKET = "labwc.sock"` supplies the nested session-control path, while KWin uses its virtual-output protocol. Pixelflux's [2.1.0 setter](https://github.com/selkies-project/pixelflux/blob/1ebcb0a10a96caeba20e1ad09be47cc018a55238/pixelflux/src/lib.rs#L7317) routes Computer-Use text and KWin relative-pointer injection to that app compositor. The [container entrypoint](https://github.com/selkies-project/selkies/blob/bcb5d67aa4c1182b48bb9a562037b8bc9092b447/addons/base/container-entrypoint.sh) chooses labwc when available, accepts another installed compositor, and documents nested ownership; the [base image](https://github.com/selkies-project/selkies/blob/bcb5d67aa4c1182b48bb9a562037b8bc9092b447/addons/base/Dockerfile) includes the patched labwc. The inner compositor manages application windows and decorations; pixelflux sees the outer compositor surface.
- **Plain:** the same resolver falls back to the capture socket. `SELKIES_WAYLAND_COMPOSITOR=none`, or an unavailable selected compositor, leaves direct Wayland clients on pixelflux without a nested window manager.
- **Host capture:** [2.1.0 `src/wayland/host.rs`](https://github.com/selkies-project/pixelflux/blob/1ebcb0a10a96caeba20e1ad09be47cc018a55238/pixelflux/src/wayland/host.rs) captures another compositor's outputs over capture protocols/portal and injects input there. In `render_node_tick`, `host_mode` adopts host frames; the local GLES/Pixman composition branches are guarded by `!host_mode`. External apps do not receive pixelflux's local toplevel configuration or frame callbacks.
- **Ours:** direct Chrome on a private output, normal window with CSD, scale 1, 1920×1080 output, 25 fps recording/capture, often no viewer. Evidence is from this host; other topology assessments below are source-based inferences unless stated otherwise.

Comparable **existing knobs**, verified in the base `lib.rs`: Python `CaptureSettings` exposes mutable fields by attribute name through `extract_settings()` (`target_fps`, `display_id`, capture dimensions, `scale`, `capture_cursor`, `use_wayland`, `wayland_host_display`, GPU selection and `cursor_size`). `WaylandBackend::new(width, height, dri_node, auto_gpu_selected, cursor_size)`, `ensure_wayland_display(...)`, `create_output(...)` and `resize_output(...)` expose compositor/output setup. The backend is process-wide and constructed once; stream settings and output-global policy are different scopes. Existing standalone environment patterns include `PIXELFLUX_APP_WAYLAND_DISPLAY`, `PIXELFLUX_RECORD_*` and `PIXELFLUX_HOST_CAPTURE`. Proposed knob names below are **inferred design locations, not existing APIs**.

### Upstream searches and filing status

Searched public issues **and PRs**, open and closed, in both projects on 2026-10-21. The exact GitHub issue-search queries were:

| Query, with `repo:R` replaced by the stated repository | Pixelflux count | Selkies count |
|---|---:|---:|
| `repo:R audio OR recorder OR timestamp` | 3 | 79 |
| `repo:R fullscreen OR decoration OR "window geometry"` | 1 | 14 |
| `repo:R "frame callback" OR "frame callbacks" OR "refresh rate" OR "presentation feedback" OR cadence` | 5 | 8 |
| `repo:R "render origin" OR "window.geometry" OR "pointer offset" OR "click offset"` | 0 | 4 |

`R` was `selkies-project/pixelflux` and `selkies-project/selkies`. Requests used `https://api.github.com/search/issues?q=<URL-encoded-query>&per_page=100`. The first attempts at the last three Selkies queries received HTTP 403; later retries returned the counts above. The three additional queries below each returned **0**:

```text
repo:selkies-project/pixelflux repo:selkies-project/selkies "AudioStartGuard"
repo:selkies-project/pixelflux repo:selkies-project/selkies "audio_thread"
repo:selkies-project/pixelflux repo:selkies-project/selkies "audio timestamps"
```

Also downloaded the public `issues?state=all&per_page=100` REST listings: pixelflux page 1 (50 records), Selkies pages 1–5 (474 records), page 6 empty. All 524 unique issue/PR titles and bodies were locally searched with:

```text
AudioStartGuard|audio_thread|audio.{0,40}anchor|capture.setup.{0,50}audio|audio.{0,50}capture.setup
normal.window|forced.fullscreen|client.side.decorat|render.origin|geometry\(\)\.loc
frame.callback|250\s*ms|presentation.feedback|capture.cadence|idle.*callback
```

These local searches covered titles/bodies, not a complete download of comments; the GitHub search requests provide the additional public search surface. Firecrawl developer-index queries were also run:

```text
selkies-project pixelflux recorder audio timestamp anchor start capture setup audio_thread
selkies-project pixelflux fullscreen normal window client side decorations window geometry render origin
selkies-project pixelflux idle frame callbacks refresh presentation feedback recording capture cadence
```

The relevant findings are linked per patch. **No exact public filing was found for any of these five local patches.** In particular, the existing [audio handoff](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/upstream/pixelflux-audio-anchor/README.md) is dated 2026-10-16 and contains no issue/PR filing link; it was reused for the audio facts below. Search results do not prove a draft, private or deleted filing never existed. No issue/PR was filed or commented on during this task.

## 1. Normal-window policy

**File:** [pixelflux-windowed.patch](https://github.com/rossrochford/pixelflux-chrome-sessions/commit/efc1c823a05b3defe102e2692653d6eda5260665)\
**SHA-256:** `f0acddc09cbaf3896e7a920feaa1c95690ba69c3cbcea2796880fca71a2bf1f3`

**Change in plain language:** When explicitly enabled, let a direct application remain a fixed-size normal window, with its own title bar and borders, instead of repeatedly forcing it fullscreen. Without the setting, the fullscreen policy remains.

**Touched code / counts:** `pixelflux/src/lib.rs` **+2/−4**: `configure_windows_for_mode()` and the `pixelflux()` Python-module initializer. `pixelflux/src/wayland/frontend.rs` **+33/−13**: new `poc_window_size()` and `apply_poc_window_policy()` helpers; `XdgDecorationHandler::{new_decoration,request_mode,unset_mode}`; initial/remap and geometry-correction paths in `CompositorHandler::commit()`; `AppState::send_forced_fullscreen_configure()`, also used by fullscreen/maximize requests. Total **+35/−17**.

`PIXELFLUX_POC_WINDOW_SIZE=WIDTHxHEIGHT` is parsed once, process-wide, through `OnceLock`. A valid positive size clears fullscreen/maximized, sets the fixed logical size and selects client-side decorations; absent/invalid input falls back to the upstream fullscreen policy. `POC_WINDOWED_POLICY` is a Python module identity marker, not another display behavior. This is not a complete window manager: interactive placement/move/resize is not implemented.

**Why needed, observed:** [WINDOW_POLICY_RESULTS.md](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/WINDOW_POLICY_RESULTS.md#window-policy-and-source-build) records forced fullscreen and the missing released normal-window knob. Its calibrated normal-window trial matched the then-visible reference's 360 compared fields, including display-mode and geometry. That older run used fractional scale; the later scale-1 [STEP3_RESULTS.md](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/hosts/intel-irisxe-ubuntu2604/STEP3_RESULTS.md) records the final 901×1016 xdg request, 945×1060 Chrome outer size and 901×929 content. The measured 44-pixel CSD/shadow correction is host/client behavior, not a universal policy constant. The latest harness notes in [PROGRESS.md](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/PROGRESS.md) additionally expose historical visible/private launch-flag differences; these older field matches do not validate the newly aligned visible launcher.

| Topology | Is unpatched behavior wrong? / reasoning |
|---|---|
| Nested | **Not generally.** Fullscreen is suitable for the outer labwc/KWin surface; the inner compositor supplies normal app windows. Enabling the local fixed-window option globally could instead constrain that outer desktop. Source-based assessment. |
| Plain | **Mode limitation, not a universal bug.** Kiosk/application streaming can intentionally fill an output; direct normal-window use cannot be represented by the existing forced-fullscreen policy. |
| Host capture | **Not applicable to captured app policy.** The external compositor configures and decorates its clients. |
| Ours | **Observed need.** Fullscreen changes geometry and browser display-mode signals relative to visible normal Chrome; the opt-in normal policy was required for the chosen comparison. |

**Default / possible setting:** Already opt-in; no need to hard-code normal mode as a new default. A named window-policy plus logical-size option could live in backend construction/output setup, with an environment fallback as this experiment uses. That scope follows `WaylandBackend`, `ensure_wayland_display`, output creation and existing process-global environment knobs, rather than a per-encoder quality field. This is a possible interface location, not an upstream recommendation.

**Dependencies:** Applies alone to base, latest and head. It references no other patch's symbols and can express normal-window policy alone. On our Chrome/CSD case, useful input/render alignment also needed the origin patch, and workflow timing needed the pacing patches; these are runtime requirements of our experiment, not textual prerequisites for window policy. Application success on latest/head was not compiled or exercised.

**Upstream relationship:** [PR #23](https://github.com/selkies-project/pixelflux/pull/23) is closed, unmerged, historical recording/Wayland architecture context, not a filing for this option. Current source still contains the fullscreen/decorations policy targeted by this patch; no exact normal-window option issue/PR was found in the searches above.

## 2. Audio timestamp anchor

**File:** [pixelflux-audio-anchor.patch](https://github.com/rossrochford/pixelflux-chrome-sessions/commit/0eb3265b70a41088b94ddb8e3e4f420f740ea79d)\
**SHA-256:** `f3dd568f23f4bc32006fc3809871dbbe8b900d57cf171cab8bf3fd9af4b839d3`

**Change in plain language:** Start consuming the live audio stream as soon as its socket connects, before potentially slow video setup. Otherwise old audio waits in the socket and is timestamped as though it arrived later, shifting the whole audio track.

**Touched code / counts:** `pixelflux/src/recorder/mod.rs`, **+83/−13**. Adds `AudioStartGuard`, `AudioStartGuard::disarm()` and `Drop`; moves audio-reader creation earlier in `recorder::start()`; adds a test-reader helper and tests `armed_guard_stops_and_joins_reader_on_drop()` and `disarmed_guard_leaves_reader_running()`. `audio_thread()`'s clock formula is not changed. The behavioral parts are early consumption of buffered live data, and ownership/cleanup of the newly early-started reader while capture setup can fail.

**Why needed, observed:** Reuses [the existing upstream handoff](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/upstream/pixelflux-audio-anchor/README.md) and [AV_RECORDING.md](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/AV_RECORDING.md). At connect, pcmflux already streams live Ogg Opus. Unpatched `start()` waits through capture setup before starting the reader; the first read sets the audio anchor. The source-build control's reference-tone audio lag was 171–215 ms, versus 35–48 ms with the patch; released-wheel runs reached 183–275 ms. The source-build A/V acceptance changed from 2/5 to 5/5; the fixed five-minute observation measured no drift. The reports retain raw probe paths, same-source-build controls, the two guard tests and the forced capture-setup failure check. These are historical measurements, not tests rerun here.

| Topology | Is unpatched behavior wrong? / reasoning |
|---|---|
| Nested | **Yes if a live recorder audio socket queues during setup.** The recorder anchors audio independently of which compositor manages application windows. Inferred from the shared recording code, not a new nested measurement. |
| Plain | **Same timestamp bug under that condition.** Window policy is unrelated to when the recorder first reads queued audio. |
| Host capture | **Same conditional bug is possible.** Backend/capture bring-up still occurs between audio connect and reader start; external app window ownership does not correct queued-audio dating. Not runtime-verified here. |
| Ours | **Measured bug**, particularly own-capture recording with setup delay. An already-active tap with very short setup can make it small, but does not change the mechanism. |

The guard is necessary because moving a thread earlier creates new failure paths after that thread starts; it is not evidence that the original no-early-thread code had this particular cleanup defect. **Code-review limit:** it protects failures before `disarm()`. The patch disarms before spawning the writer, so the existing handoff's broad error-cleanup wording must not be read as proof that every later startup failure is guarded. No new failure experiment was run for that edge.

**Default / possible setting:** The timestamp change is not a selectable app/display policy: when recording with live audio, the clock should describe that audio's capture time. Existing `RecordOptions.audio_socket`, `start_recording()` and `PIXELFLUX_RECORD_*` select whether audio is recorded. If different audio-source clock contracts were required, a `RecordOptions` timestamp/source-clock policy would be a plausible explicit interface; the inspected code does not establish a need to preserve the late-reader behavior as a default or option.

**Dependencies:** Applies and operates on base without the window, refresh, cadence or origin patches; it only uses recorder structures already present. Source-build controls in the A/V report also isolate it against the same windowed source build without the audio fix. No compile/runtime independence was newly tested in this session.

**Current upstream:** [3b549ba](https://github.com/selkies-project/pixelflux/commit/3b549ba29d59be4c87beae92ef8714b11639b78b), 2026-09-28, changes Ogg packet dating across silence-gate gaps and adds recorder tests. It is a **different** timestamp bug. Latest/head still connect audio before capture setup and spawn the reader afterward. [bcdbe59](https://github.com/selkies-project/pixelflux/commit/bcdbe59d464d9c62d56e8ccb7549770a561c1a8c) changes recording-file ownership/opening, also not this anchor fix. Rust formatting and the existing test module are additional porting considerations. Strict and fuzzy application both fail as recorded above; that failure does not establish that the anchor bug was fixed. No public filing for the local handoff/patch was found.

## 3. 59.963 Hz output and idle pacing

**File:** [pixelflux-refresh-59963.patch](https://github.com/rossrochford/pixelflux-chrome-sessions/commit/a6226fa7152a691421f95fe1dacfce872fbb81e3)\
**SHA-256:** `7cb4c53e81da9de20f3e704ab447808257c8d31d834a0448a074ea4a8fb50865`

**Change in plain language:** Describe the private output as 59.963 Hz and let idle clients receive animation callbacks about once per display frame instead of once per quarter second. It also puts that rate into initial backend timing and one presentation-feedback fallback.

**Touched code / counts:** `pixelflux/src/lib.rs`, **+16/−29**, eight hunks: top-level constants; `render_node_tick()` presentation refresh; `create_output_on()`; `resize_output_on()` fallback; `run_wayland_thread()` initial settings, primary current/preferred output modes and its no-capture timer branch.

**Parts, individually assessed:**

1. **Advertised output rate:** 60,000 to 59,963 millihertz for created/primary/preferred outputs and the resize fallback. Existing current-mode refresh is preserved by resize when present. Configuration alignment with this physical host, not a generally correct rate for every installation.
2. **Idle callback policy:** replace the 250 ms long-idle interval with `frame_period(59.963)`, remove the old distinction for parked copy sessions/recent input, and keep `frame_idle_long` false. The new `POC_REFRESH_MHZ` constant is shared with the other parts. This globally trades background work for continuously paced direct clients.
3. **Initial backend rate:** initialize `RustCaptureSettings.target_fps` to 59.963. This is initial backend/fallback timing; it does not replace an explicitly configured capture's 25 fps.
4. **Presentation fallback:** use fixed 59.963 Hz instead of `Refresh::Unknown` when rendering without a `node.capture` (for example screenshot/copy work). This line is later removed with the entire active-presentation block by capture cadence, so it has **no surviving effect in the final five-patch stack**.

**Why needed, observed:** [STEP3_RESULTS.md](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/hosts/intel-irisxe-ubuntu2604/STEP3_RESULTS.md) and the 2026-10-20 progress entries record private rAF gaps of 233.3–250 ms versus visible maxima about 16.8 ms. The intermediate build changed **both mode advertising and idle callback pacing together**; their effects were never isolated. The resulting three 600-interval private samples had maximum 16.8 ms, but their means remained around 16.666 ms, not the nominal 59.963 Hz period of about 16.677 ms. [CHROME156_RESULTS.md](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/hosts/intel-irisxe-ubuntu2604/CHROME156_RESULTS.md) preserves the smaller final visible/private mean discrepancy. Matching mode advertising therefore did not demonstrate exact callback-clock parity.

| Part | Nested | Plain | Host capture | Ours |
|---|---|---|---|---|
| Advertise 59.963 Hz | **Not generally wrong at 60 Hz.** Outer output is virtual; inner compositor owns app mode policy. A different outer rate can be useful configuration. | **Not generally wrong at 60 Hz.** Choice of virtual display mode. | External compositor owns captured app/output refresh; this local constant is not a fix for its clock. | Host-specific alignment need, but its independent effect was not measured. |
| Faster idle callbacks | Intentional upstream idle conservation can throttle an outer compositor; whether that propagates to inner app cadence depends on that compositor. **Not demonstrated as a universal bug.** | Intentional throttle; unsuitable when continuously advancing background app animation is required. | Does not drive external app callbacks; additional local timer work need not help captured apps. | Removes observed stalls in the combined intermediate change; continuous page-observable cadence is an experiment requirement. |
| Initial backend rate | Initial virtual-backend policy, not a universal correctness fix. | Same; should follow chosen output/callback policy if configured. | Not authoritative for external app clocks. | Aligns fallback timing to the chosen rate; independent benefit unmeasured. |
| Fixed presentation fallback | Meaning depends on what a virtual output calls presentation; not proven wrong universally. | Same; claiming fixed refresh is a protocol policy, not measured scanout. | Host has its own presentation/capture stamps; local fallback is not a replacement. | Offscreen timing choice, subsequently canceled by the next patch. |

These non-local assessments are **inferred**. The base code explicitly documents 250 ms as an idle-work reduction, with shorter service after input or while copy clients are attached. That intent matters when classifying the change.

**Default / possible settings:** This patch changes defaults unconditionally. An output/backend constructor argument such as `refresh_mhz` naturally belongs alongside output width/height/scale, threaded through primary and additional output setup. A distinct `idle_frame_interval_ms` or `keep_client_frames_running` backend policy would separate the resource/cadence choice. A standalone environment fallback follows existing host/app-display patterns. `CaptureSettings.target_fps` is the comparable **capture** knob, not an adequate independent output-mode knob: our recording intentionally stays at 25 fps. Presentation policy could be a separate backend enum. These are possible interface locations; none of these proposed knobs exists in the patch.

**Dependencies:** Applies alone to base and needs no other patch symbols. In the final stack its shared `POC_REFRESH_MHZ` is required by capture cadence. Latest/head application fails, although their source still advertises 60 Hz and keeps a 250 ms idle constant. No separated-part application or runtime test was performed.

**Upstream relationship:** [Selkies #402](https://github.com/selkies-project/selkies/issues/402) is a closed report about fresh-input phase and capture latency in nested labwc; early frame callbacks alone did not solve that report's problem. It does not establish a requirement for full-rate unwatched applications. [92e5db0](https://github.com/selkies-project/pixelflux/commit/92e5db06dd4aafa6ed6999b33cfb927b9c63c14f) changes pacing budgets/timer-lateness handling; [31753b7](https://github.com/selkies-project/pixelflux/commit/31753b73fb5c556d30e47888ab572bb1007c8118) preserves fractional encoder frame rates. Neither replaces the local advertised-output/idle policy. No exact 59.963 Hz or no-viewer rAF filing was found.

## 4. Independent capture cadence and presentation handling

**File:** [pixelflux-capture-cadence.patch](https://github.com/rossrochford/pixelflux-chrome-sessions/commit/1ca239b464e486537ec05b9156ad901ca045de90)\
**SHA-256:** `f52c645b6fa0154cc5b50a478c6f7934aff70a3b0f3785e80890f834c62bae02`

**Change in plain language:** Keep application animation callbacks running at the virtual output's rate while a viewer or recorder samples video at a different rate. Separately, stop reporting sampled offscreen captures as presentation events and discard those feedback requests instead.

**Touched code / counts:** `pixelflux/src/lib.rs`, **+12/−52**, five hunks. `render_node_tick()` loses the render-driven window/layer/cursor callback and presentation block. Inside `run_wayland_thread()`, `send_idle_frame_callbacks()` is reused as a general service pass; the input-wake shortcut is removed, a new independent `Timer::immediate()` source is added, and the old no-capture timer no longer sends callbacks. The actual capture timer and encoder sampling remain in place.

**Separate behavioral parts:**

- **Callback scheduling:** a process-wide timer services ownerless outputs at `frame_period(POC_REFRESH_MHZ / 1000)`, whether capture is active or not. Active capture no longer drives client callbacks. The helper also prunes the GLES texture cache, so that work now happens during active capture as well. The removed render block sent per-output cursor callbacks; the reused helper services the primary output's cursor. **Inferred limitation:** this is not a proven multi-output cursor/cadence design.
- **Presentation feedback:** the reused helper calls `feedback.discarded()` even during active capture; it replaces `feedback.presented(... Refresh::Fixed(capture_fps) ... Kind::Vsync)` in the render path. Independent callbacks and discarding presentation feedback were **not tested separately**. There is no demonstration that every virtual compositor must discard feedback rather than define a virtual presentation event.

**Why needed, observed:** [STEP5_RESULTS.md](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/hosts/intel-irisxe-ubuntu2604/STEP5_RESULTS.md#reproduced-defects-and-compositor-changes) and the 2026-10-20 preliminary progress entry record three samples per phase near 16.666 ms when idle, then about 40 ms with the 25 fps viewer/recording, reverting after detach. The derivative's six phases retained approximately 16.666 ms means and 16.8 ms maxima. The Chrome 156 repeat retains all 18 samples and successful 70-second recording in [CHROME156_RESULTS.md](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/hosts/intel-irisxe-ubuntu2604/CHROME156_RESULTS.md). That validates the combined derivative workflow, not isolated feedback semantics or a real physical presentation clock.

| Part | Nested | Plain | Host capture | Ours |
|---|---|---|---|---|
| Independent callbacks | Upstream capture-driven cadence can intentionally pace the outer compositor to the stream. Decoupling could help continuously running apps, but inner scheduling and extra work need validation. **Not universally wrong unpatched.** | App cadence following the fastest active capture can be desired for streaming efficiency, or unsuitable when recording should not slow the app. Mode-specific requirement. | External compositor supplies app callbacks; local timer does not take over its clock. No demonstrated host-app fix. | **Observed requirement:** 25 fps capture must not change page rAF to 25 fps. The combined patch removed that phase change. |
| Discard active feedback | Could affect the outer compositor's frame scheduling. Reporting a rendered virtual output as presentation is not disproved merely because it has no physical monitor. | Offscreen semantics need definition: the old synthetic `Vsync` claim warrants scrutiny, but all-feedback discard is a distinct policy choice. | External feedback belongs to the host. The helper handles local surfaces, not host apps' feedback. | Accepted offscreen experiment policy; its necessity independently of callback decoupling is unmeasured. |

Except the “ours” observations, these are **inferences from code paths**, not new topology tests. The existing report itself calls this an isolated experiment, not a general multi-output/host-capture fix.

**Default / possible settings:** The patch unconditionally changes callback/feedback scheduling for local outputs. Possible backend/output options are `client_frame_rate` or `frame_callback_policy` and a separate `presentation_feedback_policy`, provided at `WaylandBackend`/output construction with environment fallbacks for standalone use. Keep them distinct from per-stream `CaptureSettings.target_fps` and `display_id`. Existing per-output creation and capture-rate controls support that separation, but no independent per-output callback clock is implemented by this fixed-rate timer.

**Dependencies:** Does **not** apply alone to clean base: hunks 1 and 5 expect refresh-patch content, and the timer references that patch's `POC_REFRESH_MHZ`. Applying only refresh first makes its strict check pass, without window or audio. Functionally its intended rate depends on that constant; normal-window policy and origin correction are not necessary to run the scheduling code, while audio timing is independent. Current latest/head still have capture-driven callback/presentation code, but this old patch is not directly applicable there. No port or compile was attempted.

**Upstream relationship:** [Selkies #402](https://github.com/selkies-project/selkies/issues/402) concerns publication phase after input, not keeping a 60 Hz application clock during 25 fps recording. [Pixelflux PR #34](https://github.com/selkies-project/pixelflux/pull/34), merged 2026-09-25, addresses X11 vsynced-frame duplication/skipping and is another distinct pacing problem. No exact public issue/PR for this independent callback/discard policy was found.

## 5. Surface render origin

**File:** [pixelflux-window-origin.patch](https://github.com/rossrochford/pixelflux-chrome-sessions/commit/ddb6b548ca5d66e7415038173a500861013f78cf)\
**SHA-256:** `9731c9416bed0a9f412ca5e67bcbfe814e0add168ae924ea000d83662995bd8f`

**Change in plain language:** Draw a window at the same surface origin used for input, accounting for the shadow/border inset reported by the client. Otherwise the picture is shifted relative to the places that receive clicks.

**Touched code / counts:** `pixelflux/src/lib.rs`, `render_node_tick()`, **+2/−2**. The GLES and Pixman window loops both change `element_location(window) - output_origin` to additionally subtract `window.geometry().loc` before producing render elements.

**Why needed, observed:** [STEP5_RESULTS.md](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/hosts/intel-irisxe-ubuntu2604/STEP5_RESULTS.md#reproduced-defects-and-compositor-changes) records original fixture origin `(22,106)` and an intended page click `(47,55)` arriving at `(69,74)`: a reproducible 22×19 displacement. With the derivative, content starts `(0,87)` and input reaches `(47,55)`; viewer click/type/submit/reconnect pass. The shadow-related geometry offset is preserved in raw mapping and pointer traces under the host's `artifacts/workflow-20261020/` and `workflow-20261021/` directories.

The source reasoning is verified against pinned Smithay commit `5de53056b5cafd453371af2d4e955931ce87ef71`: [`InnerElement::render_location()`](https://github.com/Smithay/smithay/blob/5de53056b5cafd453371af2d4e955931ce87ef71/src/desktop/space/mod.rs) computes `location - element.geometry().loc`; [`Window::render_elements()`](https://github.com/Smithay/smithay/blob/5de53056b5cafd453371af2d4e955931ce87ef71/src/desktop/space/wayland/window.rs) uses the supplied location as the root surface's render position. Pixelflux's custom render loops bypassed that space helper. Both cached dependency source and the patch were inspected; no dependency tree was edited.

Latest/head pin Smithay `928d4a9b222b2adcd739a2c60caf3e8f304a45a5`. Its [space helper](https://github.com/Smithay/smithay/blob/928d4a9b222b2adcd739a2c60caf3e8f304a45a5/src/desktop/space/mod.rs) and [window renderer](https://github.com/Smithay/smithay/blob/928d4a9b222b2adcd739a2c60caf3e8f304a45a5/src/desktop/space/wayland/window.rs) were checked too: the geometry subtraction remains the space helper's responsibility, rather than being newly added inside `Window::render_elements()`. Thus the dependency bump does not itself explain away the local origin correction.

| Topology | Is unpatched behavior wrong? / reasoning |
|---|---|
| Nested | **Usually masked**, because the outer fullscreen compositor surface commonly has zero geometry inset and the inner compositor manages actual app decorations. If the outer surface reports a nonzero inset, the same root-origin inconsistency applies there. Inferred; no new nested test. |
| Plain | **Yes for direct surfaces with nonzero geometry offsets.** Rendering and input should agree; zero-offset fullscreen clients do not expose the error. |
| Host capture | **Not applicable to host-owned app geometry.** The host has already composed its buffers; the patched local-render branches are bypassed for that output. |
| Ours | **Measured bug** in CSD normal Chrome, corrected in the validated derivative. |

**Default / possible setting:** The origin correction is geometric consistency, not a selected display style. An opt-in compatibility/debug switch could technically be placed on backend construction, comparable to the existing render-backend/GPU selection, but no inspected setting provides a reason for intentionally mismatching render/input origins. There is no need to invent a per-encoder `CaptureSettings` quality option for this calculation. This describes interface feasibility, not a decision to upstream it.

**Dependencies:** Applies alone to clean base; no symbols from any other local patch. Nonzero geometry is what makes it matter, not the window patch itself, although our normal-window option exposes that case. It changes both renderer branches; only this host's actual workflow was measured. The base's existing separately drawn popup duplication is not fixed by this two-line patch.

**Current upstream / conflicts:** [d6a1939](https://github.com/selkies-project/pixelflux/commit/d6a1939b2bdf09113a90ef706c444c5b4ba299f4), 2026-10-02, removes duplicate manually rendered popups and fixes their shadow-offset placement. That deletes the context immediately following both local origin hunks, explaining application failures. It leaves the root-window `element_location - origin` calculation in place: **related code changed, but it is not this root-surface subtraction**. [4f7469b](https://github.com/selkies-project/pixelflux/commit/4f7469b7382f29df4ce5203a0867a6299ccc554e) additionally fixes layer popups and popup input. [Selkies #320](https://github.com/selkies-project/selkies/issues/320) concerns cross-display browser-window coordinate conversion, not this direct CSD root origin. No exact public filing for the two-line correction was found.

## Build and distribution facts

**Artifact and build:** `artifacts/windowed-build/build-module-workflow.json` records `cargo build --locked --lib -j 4`, `PYO3_PYTHON=.../experiments/gpu-display/.venv/bin/python`, `CARGO_PROFILE_DEV_DEBUG=0` and `CARGO_PROFILE_DEV_OPT_LEVEL=1`. This is the **dev profile**, not `--release`; debug information is disabled but release optimizations/LTO are not selected. The retained Cargo fingerprint reports `["default", "gpl"]`. `build_windowed.py` copies `pixelflux/target/debug/libpixelflux.so` into `artifacts/windowed-build/module-workflow/pixelflux.cpython-314-x86_64-linux-gnu.so`. `session.py:Session.__init__()`/`start()` give only the worker this explicit module directory through `PYTHONPATH` and verify the loaded module path; this is an isolated module override, not replacement of the installed wheel. The helper's import/marker check is part of the recorded build workflow, not something run during this audit.

**Encoder implementation in the base:** `encoders/software.rs` supplies striped JPEG and software H.264; default Cargo `gpl` selects `x264-sys`/libx264. `openh264` is the non-default alternative, normally selected by `PIXELFLUX_ENABLE_GPL=0` in `setup.py`, with `--no-default-features --features openh264`. `encoders/avcodec.rs` reaches FFmpeg software H.265/VP8/VP9/AV1 and VA-API; `nvenc.rs` uses NVIDIA's runtime driver APIs; `v4l2m2m.rs` is the generic kernel encoder path. Tegra is `aarch64`-only and is not included on this x86_64 host. Presence of modules/libraries is not evidence that this Intel run used them: the workflow selected software **H.264/x264 at 25 fps**, with pcmflux-provided Opus.

**Measured linking:** `readelf -d` on the derivative reports these 13 direct `NEEDED` libraries, with **no RPATH/RUNPATH**:

```text
libx264.so.165       libavutil.so.60      libavfilter.so.11
libavcodec.so.62     libgbm.so.1          libudev.so.1
libinput.so.10       libpixman-1.so.0     libxkbcommon.so.0
libgcc_s.so.1        libm.so.6            libc.so.6
ld-linux-x86-64.so.2
```

`ldd` resolves all of them against the host; its full transitive graph has 171 `=>` entries and no missing libraries. Relevant codec dependencies include system libx264, x265, libvpx, SVT-AV1 and dav1d plus FFmpeg's avformat/swscale/swresample. This actual dev binary also directly retains libinput/libudev, despite the base `LICENSES.md` describing them as graph-only/dropped for its audited build; the binary measurement takes precedence for this artifact. Runtime-loaded EGL/Wayland/NVIDIA/PipeWire libraries are a separate category and need not appear in `NEEDED`.

**Released-wheel comparison:** retained `artifacts/wheels/pixelflux-2.1.0-cp314-cp314-manylinux_2_28_x86_64.whl` has SHA-256 **`a4f3fb52f5e2dec03c9b1539ac750c19d43a04d00b4de744054260292d1da0f2`**, matching the PyPI listing. It bundles 12 shared libraries under `pixelflux.libs/`, with auditwheel hash-suffixed names, and the extension has `RPATH=$ORIGIN/pixelflux.libs`:

| Library family | Derivative resolves from host | Released 2.1.0 wheel bundles |
|---|---|---|
| x264 / x265 | `libx264.so.165` / `libx265.so.215` | `libx264-03b89520.so.165` / `libx265-01f14378.so.216` |
| FFmpeg avcodec / avfilter / avutil | 62.11.100 / 11.4.100 / 60.8.100 | 62.28.100 / 11.14.100 / 60.26.100 |
| FFmpeg avformat / swscale / swresample | 62.3.100 / 9.1.100 / 6.1.100 | 62.12.100 / 9.5.100 / 6.3.100 |
| libvpx / SVT-AV1 / dav1d | 12.0.0 / 2.3.0 / 7.0.0 | 11.0.1 / 4.2.0 / 7.0.0 |
| kvazaar | Not found in this artifact's `ldd` graph | `libkvazaar-bff795db.so.7.4.0` |

GPU/display libraries remain host-provided. These distribution differences prevent attributing encoder-performance differences between wheel and derivative solely to patches.

**Licences, as declared by the sources:** pixelflux itself is [MPL-2.0](https://github.com/selkies-project/pixelflux/blob/1ebcb0a10a96caeba20e1ad09be47cc018a55238/LICENSE), also recorded in the wheel metadata. The base [third-party inventory](https://github.com/selkies-project/pixelflux/blob/1ebcb0a10a96caeba20e1ad09be47cc018a55238/LICENSES.md) identifies libx264 and x265 as GPL-2.0-or-later; FFmpeg's licence depends on its build configuration, with GPL components in the published GPL wheel/system GPL builds. OpenH264 is the alternative H.264 feature, not the feature of this derivative. These are component/build facts, not a distribution-licensing decision.

**What a reproducible release build would additionally need:** a fresh verified tag-archive extraction, the recorded five hashes/order and lockfile; pinned Rust/C/C++/assembler/Python build toolchains and system/codec inputs or a pinned wheel-build image; an explicit GPL/non-GPL feature choice; and a separately named output built with the release packaging recipe. The base `Cargo.toml` release profile is `opt-level=3`, `lto="fat"`, `codegen-units=1`, `strip=true`; `setup.py` uses `debug=False`, strips the extension and passes `--locked`. Reproducing a portable wheel also requires the pinned codec build and auditwheel repair/exclusions in `pyproject.toml`, ABI/platform tags, bundled-library inventories and licences, and a new artifact hash with runtime validation. The current helper's source-directory reuse and host-library linkage are not sufficient for bit-for-bit release reproducibility. No such build was attempted.

For any port to **2.2.1** or later (none is currently planned), packaging is also not mechanically identical: [b88608b](https://github.com/selkies-project/pixelflux/commit/b88608b0191c43ad73e2bab7d0c6c04f70c3b60c) replaced FFmpeg-backed encoder integration with direct codec/libva modules after the base. Successful patch application alone would not validate a migrated encoder/build stack.
