# Fork notes

This is an unofficial fork of [sintaxerrorpvtltd/Open-Tangerine](https://github.com/sintaxerrorpvtltd/Open-Tangerine).
It is GPL-3.0-only, like upstream, and keeps upstream's attribution and third-party notices.

## The only change: a build fix

`scripts/build-app.sh` now pins the Swift deployment target:

```sh
MACOS_TARGET="${MACOS_TARGET:-$(uname -m)-apple-macos26.0}"
xcrun swiftc ... -target "$MACOS_TARGET" ...
```

**Why.** With a Swift toolchain newer than the bundle's `LSMinimumSystemVersion`,
`swiftc` stamps the *next* major as `minos`. Building upstream at `1fc2333` on
macOS 27 with Swift 6.3.3 produced `minos 28.0`. The raw binary still ran, but
LaunchServices refused the app bundle:

```
open "dist/Open Tangerine.app"
_LSOpenURLsWithURLsWithCompletionHandler() failed with error -10825   # kLSNoExecutableErr
```

Pinning the target to `26.0` (the value in `Info.plist`) makes `open` succeed and
keeps the binary installable on the advertised minimum OS. Override with
`MACOS_TARGET=arm64-apple-macos27.0 ./scripts/build-app.sh` if you prefer.

## What is *not* changed

- No UI, artwork, animation, radial wheel or notch code was touched.
- Bundle identifier, version and icon stay upstream's.
- No upstream feature was removed and no new feature was added.

## Build

```sh
./scripts/build-app.sh
open "dist/Open Tangerine.app"
```

## Upstream

Report upstream bugs and feature requests at
<https://github.com/sintaxerrorpvtltd/Open-Tangerine/issues>.
