---
title: More repositories and Macs
weight: 5
---

## Several repositories on one Mac

Click **Add Repository…** in the app's sidebar and give the repository's
address. If your GitHub App is not installed on it yet, GitHub opens so
that you can add it.

- The repositories share the Mac's VMs, first come, first served. macOS
  allows two macOS VMs at once, so two jobs run at a time.
- Each repository needs its own `.chorus/image.yaml` and lock. Two
  repositories whose files match share one image.
- Each has its own network mode and its own caches.

{{< shot name="repository-changes" height="900" alt="A repository's page, with two of its settings changed and waiting to be applied." caption="A repository's page. Settings are changed freely and applied together, with one restart of the daemon. This page is in the release after 0.23.0." >}}

Chorus serves private repositories you administer. It does not serve
public ones, where anyone can open a pull request that would run on
your Mac.

## Sharing jobs between Macs

Set up a second Mac the same way as the first. Then open the
repository's page in its sidebar and, under **Sharing Jobs with Other
Macs**, enter the label your workflows ask for: the first Mac's
`runs-on` label. GitHub gives each job to one of the Macs.

Each Mac has its own GitHub App, builds its own image and keeps its own
caches. If a Mac's library lacks the Xcode the lock pins, the app says
which one it needs and takes the `.xip` once you have downloaded it.

## Taking a Mac out for a while

**Stop Taking Jobs** in the app, or `chorus drain`, lets running jobs
finish and takes no new ones, so the other Macs carry the load.
`chorus resume` ends it.
