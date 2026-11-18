---
title: The image
weight: 2
---

What a job's VM contains is declared in `.chorus/image.yaml` in your
repository. `.chorus/image.lock`, written by `chorus image lock`, pins
exactly what that resolved to. Nothing gets into an image except through
these two files.

Chorus reads both from the repository's default branch and nowhere else.
A pull request cannot change what is installed for its own job; a change
to the manifest takes effect after it merges.

## The manifest

```yaml
version: 1

base:
  macos: "27"              # the lock pins the exact build

xcode:
  - version: "27"          # the lock pins the build
    platforms:
      - ios: "27"          # simulator runtimes to install
    simulators:            # devices that must exist
      - iPhone 17 Pro

brew:                      # Homebrew formulae
  - xcbeautify

caches:                    # cache volumes to mount in every job
  - xcode
  - swiftpm
```

| Key | What it declares |
| --- | --- |
| `base.macos` | The VM's macOS version. |
| `xcode[].version` | An Xcode to install, at `/Applications/Xcode_<version>.app`. List more than one to have several side by side. |
| `xcode[].platforms` | Simulator runtimes for that Xcode: `ios`, `watchos`, `tvos` or `visionos`, each with a version. |
| `xcode[].simulators` | Device names that must exist. The build fails if one does not. |
| `brew` | Homebrew formulae. The lock records each one's version and checksum, and those of the formulae it needs. |
| `caches` | Cache volumes; see [Caches](../caches). |

The manifest also accepts `tools` (release binaries from GitHub, pinned
by tag and checksum), `defaults` (preference values for the runner's
user) and `run` (shell steps). These are newer and less tried than the
keys above.

## Changing the image

1. Edit `image.yaml` on a branch.
2. Run `chorus image lock -C /path/to/your/checkout` on a Mac that runs
   Chorus. It rewrites `image.lock`.
3. Commit both files and merge them to the default branch.
4. Chorus notices within a few minutes and builds the new image.

Jobs keep using the old image until the new one is ready. A build reuses
every layer that did not change, so adding a tool takes minutes, not the
hour of a first build.

## Where Xcode comes from

Apple requires a signed-in account to download Xcode, so Chorus never
downloads it. Download the `.xip` from
[Apple's developer site](https://developer.apple.com/download/all/) and
add it to the Mac's library, from the app or with:

```
chorus xcode add ~/Downloads/Xcode_27.xip
```

No Apple credential ever enters a VM. macOS itself, simulator runtimes
and Homebrew formulae are fetched from Apple's and Homebrew's public
servers.
