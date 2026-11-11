# Umbra Sigil releases

Signed, notarised Apple-silicon macOS builds of Umbra Sigil, a local TOTP/HOTP
authenticator, and the signed metadata its manual update check reads. This
repository contains no source code. Umbra Sigil is proprietary software; see the
`LICENSE` and `THIRD-PARTY-NOTICES` files inside the app bundle.

## Requirements

Apple silicon and macOS 15.7.7 or later.

## Verifying a download

Check the DMG against the release's `SHA256SUMS`:

```sh
shasum -a 256 -c SHA256SUMS
```

Then confirm Apple's notarisation and the Developer ID team `N7447R8KW2`:

```sh
spctl -a -vv -t open --context context:primary-signature Umbra-Sigil_<version>_aarch64.dmg
spctl -a -vv -t exec "/Applications/Umbra Sigil.app"
```

Both should report `source=Notarized Developer ID` and
`origin=Developer ID Application: KIAN PHILIP WATKINS (N7447R8KW2)`.

## Update metadata

The app checks for updates only when you ask it to, in Settings. It downloads
`sigil-update.json` from the `updates-beta` release and verifies an Ed25519
signature against the public key built into the app:

```text
bff9dc9222c64dd126c56fc229c33c3ff9a26783f006aaacb3cfce3edb624a0b
```

The metadata names a version only. Nothing is downloaded or installed
automatically. Signed metadata expires after seven days, and an expired or
missing file is reported as an error, not as "up to date".

## Upgrading

Upgrades are manual. Before replacing the app, make an encrypted export and keep
it, with its password stored separately, off this Mac. Keep the previous app
until the new one has opened your vault.
