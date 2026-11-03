# NoManos releases

Public home for NoManos update artifacts. The NoManos source lives in a separate private repository;
this repository holds only what installed apps need to update themselves with
[Sparkle](https://sparkle-project.org).

| What | Where |
| --- | --- |
| Update feed (`SUFeedURL`) | `https://raw.githubusercontent.com/ianboru/NoManos-releases/main/appcast.xml` |
| Release archives | `https://github.com/ianboru/NoManos-releases/releases/download/v<version>/NoManos-<version>.zip` |

## Contents

- `appcast.xml`: the Sparkle feed. Each item names a version, its build number, the archive URL,
  its size, and an EdDSA signature that installed apps check against the `SUPublicEDKey` bundled in NoManos.
- GitHub Releases `v<version>`: one notarized `NoManos-<version>.zip` per release.

## Publishing

Don't edit this repository by hand. Releases are produced from the NoManos source repository:

```bash
scripts/publish-release.sh --check     # what's ready and what's missing
scripts/publish-release.sh --no-build --publish \
  --developer-id "Developer ID Application: <Name> (<TEAM>)" --notary-profile <profile> --notes notes.md
```

That script builds, signs, notarizes, zips, and signs the archive. It then uploads the ZIP to a GitHub Release
here first, commits and pushes `appcast.xml` second, and checks both URLs without authentication.

## Rules

- This repository must stay **public**: Sparkle fetches the feed and archives without credentials.
- Never replace a published archive with different bytes under the same URL. Ship a new version instead.
- Build numbers only go up: Sparkle offers an item only when its build is newer than the installed one.
- Keep the feed URL stable. Installed apps only know the URL they were built with.
