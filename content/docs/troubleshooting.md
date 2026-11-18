---
title: Troubleshooting
weight: 6
---

## What to check

```
chorus doctor       # what this Mac lacks
chorus status       # the daemon, its image and its VMs
chorus image list   # image layers and the last build
chorus cache list   # cache volumes, and why the last one was kept or not
```

The daemon's log is `~/Library/Logs/Chorus/chorusd.log`.

## Common causes

- **A job waits and never starts.** Chorus runs in your login session:
  jobs wait while the Mac is asleep or logged out. The overview also
  says when an image is still building.
- **A step fails downloading something.** In GitHub only mode a job
  reaches GitHub and nothing else. See [Network](../network).
- **A job stops partway.** A VM is force-stopped after its time limit,
  which the overview shows.
- **The image will not build.** The overview's **Needs You** list says
  what is missing, such as an Xcode archive the lock pins.

## Updating

Chorus looks for a new version once a day and offers it when there is
one; Settings has the switch, and **Check for Updates…** looks now. After
an update Chorus restarts its daemon by itself, once no job is running
and no image is being built. This is in Chorus 0.22.0 and later; an
older one is updated by hand, once.

To update by hand, quit Chorus, replace `Chorus.app` with the new one and
open it. The daemon is still the old one at that point, and the overview
says so: click **Restart Daemon**. If jobs are running it waits for them
to finish first, taking no new ones meanwhile.

`chorus version` shows the tool's build and the running daemon's.

## Removing

Click **Stop Daemon** in Chorus's Settings, delete `Chorus.app`, delete
the VMs whose names start with `chorus-` (`tart list` shows them,
`tart delete <name>` removes one), delete
`~/Library/Application Support/Chorus`, and uninstall the GitHub App
from your repository's settings.

## Still stuck

[Open an issue](https://github.com/bmorton/ChorusApp/issues) with what
`chorus doctor` and `chorus status` print.
