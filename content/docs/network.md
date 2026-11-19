---
title: Network
weight: 3
---

What a job may reach is decided on the Mac, outside the VM, where
nothing a job runs can change it. In every mode a job cannot reach the
Mac itself, your local network, or the other VM.

You choose one of two modes for each repository, on its page in the app.

| Mode | A job can reach | Good for |
| --- | --- | --- |
| Restricted | GitHub, and what you tick | Everything, once you know what a job needs. This is the default. |
| Observe | Any public address, each one recorded by name | Finding out what a job needs |

## Restricted: groups you tick

In Restricted mode a job reaches GitHub, which it cannot run without,
and nothing else until you tick it. Each box is a group of names that
belong together:

| Group | Tick it when a job |
| --- | --- |
| GitHub | Always on |
| Apple developer services | Uses App Store Connect or TestFlight, or downloads from Apple |
| Signing and notarizing | Releases a signed app |
| Claude Code | Runs the agent |

A repository you add starts with GitHub alone, which is enough for most
test jobs. The sheet that adds a repository offers the same boxes.

From the command line:

```
chorus repo set -groups apple,signing <runs-on label>
chorus repo set -groups none <runs-on label>
```

## What each group admits

Chorus is opinionated here: a group admits what its purpose ordinarily
asks for, and nothing beyond that.

**GitHub**, always:

| Names | Port | What for |
| --- | --- | --- |
| GitHub's runner endpoints | 443 | Fetching the job, actions, artifacts, releases |
| `uploads.github.com` | 443 | Publishing a release's files from a job |

**Apple developer services**:

| Names | Port | What for |
| --- | --- | --- |
| `apple.com`, `*.apple.com`, `*.cdn-apple.com` | 443 | App Store Connect, TestFlight, provisioning, Xcode's downloads |

iCloud's own names are left out on purpose.

**Signing and notarizing**:

| Names | Port | What for |
| --- | --- | --- |
| `www.apple.com` | 443 | Apple's Developer ID intermediate certificates, which a release job downloads before it signs. This admits Apple's website as a whole: the filter is by name. |
| `appstoreconnect.apple.com` | 443 | Apple's notary service |
| `api.apple-cloudkit.com` | 443 | The notarization ticket that `stapler` fetches |
| `notary-submissions-prod.s3-accelerate.amazonaws.com` | 443 | The upload `notarytool` makes: a storage bucket of Apple's at Amazon, and the one name here that is not Apple's own |
| `timestamp.apple.com`, `ocsp.apple.com`, `ocsp2.apple.com` | 443 | Timestamps and certificate checks |
| `timestamp.apple.com` | 80, unencrypted | The timestamp a Developer ID signature needs before it can be notarized |
| `ocsp.apple.com` | 80, unencrypted | Asking Apple whether a certificate has been revoked |

Apple serves the two unencrypted ones no other way; what they answer is
signed by Apple, so it does not depend on the connection being private.
If a release still stops with this group ticked alone, tick Apple
developer services as well, and tell us which name it was.

**Claude Code**:

| Names | Port | What for |
| --- | --- | --- |
| `api.anthropic.com` | 443 | The agent's API |
| `claude.ai`, `claude.com`, `platform.claude.com` | 443 | Signing in |
| `downloads.claude.ai` | 443 | Installing and updating it |
| `code.claude.com` | 443 | Its documentation |

Left out on purpose, for you to allow if you want them: the package
registry it installs plugins from, its telemetry, and connectors from
claude.ai.

Chorus's Help menu opens this page.

## Allowing more names

In Restricted mode you can also allow hostnames of your own for a
repository, such as a package registry: on the repository's page in the
app, or with

```
chorus repo set -allow registry.example.com,*.example.org <runs-on label>
```

What is ticked and what is allowed are kept on your Mac. Neither ever
comes from the repository or from a job, so a workflow cannot widen its
own access.

## Observe

In Observe mode a job reaches any public address, and what it asks for
is recorded by name. Use it to find out what a job needs, then go back
to Restricted and tick only that. Software that ignores proxy settings
connects directly and is not recorded.

## Seeing what a job asked for

{{< shot name="vm-traffic-hosts" alt="A job's Traffic tab: each host it asked for, whether it was allowed or refused, and how much went each way." >}}

The app's Traffic tab shows, for each job, the names it asked for and
the ones that were refused. The page of a job that failed lists what it
was refused, and which box would admit each name. A fresh macOS asks
for Apple names all the time, whatever the job does, so some refusals
are not the job's. Either put what a step needs into
[the image](../image), tick its group, or allow the name.

## What the modes do not promise

Any service that accepts uploads is a way for data to leave. GitHub is
one in every mode, and so is every group you tick and every name you
allow. The modes limit where a job can send what it can read;
they do not make an untrusted job safe to give secrets to.
