# Fork migration validation — 2026-10-27

The five commits listed in [PATCHES.md](../PATCHES.md) import the existing
validated 2.1.0 patches without source changes. The source commit is
`ddb6b548ca5d66e7415038173a500861013f78cf`, on `chrome-sessions-2.1.0`, based on
upstream `1ebcb0a10a96caeba20e1ad09be47cc018a55238` (tag `2.1.0`).

## Source identity

Before adding documentation, the checkout and retained validated source had
identical sets of 93 non-generated files and identical bytes in every file.
Only these three files differ from the upstream base:

- `pixelflux/src/lib.rs`
- `pixelflux/src/recorder/mod.rs`
- `pixelflux/src/wayland/frontend.rs`

After documentation, all 92 original files other than README still match the
reference. README differs only by the authorized fork note; the upstream text is
preserved. The five patch hashes match the original build provenance.
`pixelflux/Cargo.lock` remains SHA-256
`d3023efa7a99e21411b35026d03bc7dbadaab3f0ebe940a4110c691bd210ef76`.
The retained reference source, target directory, experiment configurations and
native module were not overwritten. The retained module still hashes to
`215b3a04a4f1d9ae1a0f839d314221101fc8f7dfc93199452fa58a8765e8e8ab`.

## Fresh native checks

Run on `ross-ThinkPad-X1-Carbon-Gen-10`, Ubuntu 26.04.1, x86_64 Intel Iris Xe,
using the existing host toolchain: Cargo/Rust 1.93.1, CPython 3.14.4 and host
FFmpeg (`libavcodec` 62.11.100, `libavfilter` 11.4.100), with x264 0.165.
No packages or Python environments were installed. The target directory is
isolated from the retained experiment build.

From the fork's `pixelflux/` directory, the shared environment is:

```bash
export PYO3_PYTHON=/home/ross/code/chrome-sessions/experiments/gpu-display/.venv/bin/python
export CARGO_TARGET_DIR=/home/ross/code/pixelflux-chrome-sessions/target-fork-migration
export CARGO_BUILD_JOBS=4
export CARGO_PROFILE_DEV_DEBUG=0
export CARGO_PROFILE_DEV_OPT_LEVEL=1
```

The two feature configurations run in sequence:

```bash
cargo test --locked --lib
cargo test --locked --lib --no-default-features --features openh264 -- --test-threads=1
```

The OpenH264 test threads run serially because the retained workflow report
records a parallel-only decode failure. Both fresh commands exited successfully:

| Feature configuration | Passed | Failed | Ignored |
|---|---:|---:|---:|
| Default / GPL | 273 | 0 | 45 |
| OpenH264, serial test threads | 268 | 0 | 45 |

Both audio guard tests pass in each configuration. Both builds report the
existing unused `Refresh` import at `pixelflux/src/lib.rs:67`; it is retained to
preserve the exact validated source. No test or source edits were needed.

Raw command output and identity records remain locally under chrome-sessions'
ignored `experiments/gpu-display/hosts/intel-irisxe-ubuntu2604/artifacts/fork-migration-20261027/`:
`cargo-test-gpl.log`, `cargo-test-openh264.log`, `native-checks.json`,
`doc-links.json` and `source-equivalence.json`.
Ignored artifacts are local evidence, not published attachments.

## Retained runtime evidence and limits

The [aligned controls](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/hosts/intel-irisxe-ubuntu2604/ALIGNED_CONTROLS_20261021.md)
contain three samples per side for local fields, Sannysoft and CreepJS, with zero
stable unaccepted differences and zero unexpected launch differences. The
[aligned workflow](https://github.com/rossrochford/chrome-sessions/blob/4d78dcfc64d5863b0abcf2d6e6e5c3cd1ab2c773/experiments/gpu-display/hosts/intel-irisxe-ubuntu2604/ALIGNED_WORKFLOW_20261021.md)
covers viewer input/reconnect, profile persistence, private audio routing,
recording continuity/A/V, cleanup and six-phase cadence on the identical source.
These are retained measurements with the original module and Chrome 156, not
runtime tests of a newly compiled artifact in this checkout. Their approved
host cadence tolerance and GPU sandbox limitation remain unchanged.

No wheel was built or installed, and the full Selkies sandbox transport/browser
matrix was not rerun. No new browser/compositor/viewer session or display
experiment was launched. NVIDIA/CUDA/NVENC/NvFBC and Tegra/V4L2 hardware coverage
is unavailable on this Intel host; ignored GPU tests and X11 DRI3/root-painting
checks were not run. Other hosts, nested/host-capture/multi-output configurations,
and portability of a newly packaged wheel remain unvalidated. The OpenH264
feature check uses the existing system codec stack; it does not qualify a
GPL-free binary distribution. The host FFmpeg advertises x264, x265, libvpx,
libvpx-vp9 and SVT-AV1, but not `libkvazaar`, so the fork AGENTS.md requirement
for OpenH264 tests against a kvazaar-enabled FFmpeg is not met by this host.
No replacement codec stack was constructed. See [FORK_LICENSE.md](FORK_LICENSE.md).

## Documentation and handoff

The historical patch inventory is maintained at [PATCHES.md](../PATCHES.md), and
the license review at [FORK_LICENSE.md](FORK_LICENSE.md). Their old chrome-sessions
paths forward here. Integration policy, upgrade instructions, collector,
acceptance rules, configs, host reports, patch files and upstream drafts remain
in chrome-sessions. Its README is unchanged. New and repaired documentation
links and anchors were checked against the local repositories and pinned Git objects; existing external
upstream links were preserved without a new network availability check.

The operator will push `chrome-sessions-2.1.0` manually. No push was attempted.
The chrome-sessions dependency and selected module path still use the existing
validated artifact. Dependency adoption, version upgrades and binary release
packaging are separate work.
