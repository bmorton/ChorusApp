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
add it to the Mac's library: drop the file on the app's **Library**
page, or run

```
chorus xcode add ~/Downloads/Xcode_27.xip
```

## The Library

{{< shot name="library" alt="The Library page: Xcode archives, simulator runtimes and macOS restore images, each with its build, the repositories that use it and its size, and each repository's image." >}}

**Library**, under This Mac in the app's sidebar, shows what the Mac
keeps to build images from: Xcode archives, saved simulator runtimes,
the macOS restore images Chorus fetched, and each repository's image.
Each row says which repositories use it and how big it is.

- **Add** an Xcode archive or an exported simulator runtime with the
  button in the toolbar, or by dropping it on the page.
- The **…** on a row can show the file in the Finder, check it against
  its checksum, or remove it. Something a repository's lock pins cannot
  be removed; change that repository's image first.
- When a repository's lock pins an Xcode the Mac does not have, the page
  says so at the top, and the sidebar shows a count beside Library.

From the command line: `chorus xcode list`, `chorus xcode remove 27.1`,
`chorus xcode verify 27.2`, `chorus runtime list` and
`chorus runtime remove ios 26.5`.

## Moving to a new Xcode

When Apple releases an Xcode you want your jobs to use:

1. Download its `.xip` from Apple and add it to the Library.
2. In the repository, change the version in `.chorus/image.yaml`:

   ```yaml
   xcode:
     - version: "27.3"
   ```

3. Run `chorus image lock` in the repository's checkout on the Mac, and
   merge the manifest and the lock to the default branch.
4. The repository's page in the app offers the build. macOS is reused;
   Xcode and what sits on it are built again. Jobs keep running on the
   old image until the new one is ready.

On a row of the Library, **Use in a Repository…** shows these steps
with the lines to copy. The change in the repository is yours to make:
an image is built from a file on the default branch and from nowhere
else, so it changes by a change that was reviewed there. Once no
repository uses the old Xcode, remove it from the Library to get its
space back. Each Mac that serves the repository needs the archive in
its own Library.

No Apple credential ever enters a VM. macOS itself, simulator runtimes
and Homebrew formulae are fetched from Apple's and Homebrew's public
servers.
