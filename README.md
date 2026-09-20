# Spek for Apple Silicon

Unofficial Apple Silicon (arm64) builds of [Spek](https://github.com/alexkay/spek),
an acoustic spectrum analyser.

**This project is not affiliated with upstream.** It exists only to compile
tagged Spek source on GitHub Actions and attach a zip to a GitHub Release.

## Copyright

**Copyright of Spek and of the published binaries belongs to Alexander
Kojevnikov and contributors.** Spek is licensed under the GNU GPL v3.0 or
later. See [NOTICE](NOTICE) and the [upstream LICENSE](https://github.com/alexkay/spek/blob/master/LICENSE).

This repository vendors build scripts (MIT; see [LICENSE](LICENSE)) and
produces a compilation of the requested upstream tag. It does not relicense
Spek.

## What it produces

`spek-<version>-macos-arm64.zip` containing `Spek.app`.

- Architecture: Apple Silicon (arm64)
- Minimum macOS: 10.15 (from upstream `Info.plist`)
- Ad-hoc signed, **not notarized**. If Gatekeeper blocks the app, right-click
  `Spek.app` and choose Open.

## How to publish a build

1. Push this repository to GitHub and enable Actions.
2. Actions → **Release Apple Silicon** → Run workflow.
3. Set **version** to an upstream git tag, for example `v0.8.5`.

The workflow clones `alexkay/spek` at that tag, applies
[patches/ffmpeg8-fft.patch](patches/ffmpeg8-fft.patch) (Homebrew’s FFmpeg 8
compatibility change; skipped if already present), builds `Spek.app`, and
creates a GitHub Release on **this** repository with the same tag name.

## Local build (Apple Silicon Mac with Homebrew)

```sh
git clone https://github.com/alexkay/spek.git upstream
git -C upstream checkout v0.8.5
git apply --directory=upstream patches/ffmpeg8-fft.patch || true
./scripts/bundle-spek.sh "$PWD/upstream"
```

The zip is written to `dist/`.

## Patches

`patches/ffmpeg8-fft.patch` matches
[alexkay/spek@df840257](https://github.com/alexkay/spek/commit/df8402575f1550d79c751051e9006fd3b7fa0fe0)
([PR #338](https://github.com/alexkay/spek/pull/338)). Current Homebrew
`ffmpeg` is 8.x and dropped `av_rdft_*`. The same patch is used by Homebrew,
NixOS, and FreeBSD.
