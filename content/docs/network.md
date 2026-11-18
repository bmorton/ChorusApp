---
title: Network
weight: 3
---

What a job may reach is decided on the Mac, outside the VM, where
nothing a job runs can change it. In every mode a job cannot reach the
Mac itself, your local network, or the other VM.

You choose a mode for each repository, on its page in the app.

| Mode | A job can reach | Good for |
| --- | --- | --- |
| GitHub only | GitHub's own services, by name | Test jobs. This is the default. |
| Restricted | GitHub, Apple's developer services, and names you allow | Jobs that sign and ship: TestFlight, notarization |
| Observe | Any public address, each one recorded by name | Finding out what a job needs |
| Open | Any public address | Jobs that need the wider internet |

## Allowing more names

In Restricted mode you can allow further hostnames for a repository,
such as a package registry: on the repository's page in the app, or with

```
chorus repo set -allow registry.example.com,*.example.org <runs-on label>
```

The list is kept on your Mac. It never comes from the repository or
from a job, so a workflow cannot widen its own access.

## Seeing what a job asked for

The app's Traffic tab shows, for each job, the names it asked for and
the ones that were refused. A step that fails because a download was
refused shows up there. Either put what it needs into
[the image](../image), or allow the name.

## What the modes do not promise

Any service that accepts uploads is a way for data to leave. GitHub is
one in every mode, Apple's services are in Restricted, and so is every
name you allow. The modes limit where a job can send what it can read;
they do not make an untrusted job safe to give secrets to.
