---
title: Chorus
layout: hextra-home
---

{{< shot name="overview" alt="The Chorus window: two job VMs, one running unit tests and one starting up, above a chart of what Chorus keeps on the Mac's disk." >}}

<div class="hx:mt-6 hx:mb-6">
{{< hextra/hero-headline >}}
  Your Mac, as a GitHub Actions runner
{{< /hextra/hero-headline >}}
</div>

<div class="hx:mb-12">
{{< hextra/hero-subtitle >}}
  A fresh macOS VM for every Xcode job, built from a file in your
  repository, with its network filtered on the Mac you own.
{{< /hextra/hero-subtitle >}}
</div>

<div class="hx:mb-6">
{{< hextra/hero-button text="Get Started" link="docs/getting-started" >}}
</div>

<div class="hx:mt-6"></div>

{{< hextra/feature-grid >}}
  {{< hextra/feature-card title="Getting Started" subtitle="From nothing to one job running in a Chorus VM." link="docs/getting-started" >}}
  {{< hextra/feature-card title="The Image" subtitle="Say which macOS, Xcode, simulators and tools a VM holds." link="docs/image" >}}
  {{< hextra/feature-card title="Network" subtitle="What a job may reach, and how to see what it asked for." link="docs/network" >}}
  {{< hextra/feature-card title="Caches" subtitle="Warm builds that only your default branch can update." link="docs/caches" >}}
  {{< hextra/feature-card title="More Repositories and Macs" subtitle="Serve several repositories, or share jobs between Macs." link="docs/more" >}}
  {{< hextra/feature-card title="Troubleshooting" subtitle="What to check, and how to update or remove Chorus." link="docs/troubleshooting" >}}
{{< /hextra/feature-grid >}}

<div class="hx:mt-16"></div>

## What it is like to use

{{< tabs >}}
{{< tab name="Where a job's time went" selected=true >}}
Every job gets a profile: how long each stage took, from clone to destroy, and whether the VM was short of CPU or memory.

{{< shot name="vm-succeeded-profile" alt="A finished job's profile: what stood out, a timeline from clone to destroy, and measurements of CPU, memory, disk and downloads." >}}
{{< /tab >}}
{{< tab name="What a job tried to reach" >}}
The Mac decides what a job may reach, and shows you every name it asked for and what was refused.

{{< shot name="vm-traffic-hosts" alt="A running job's traffic, by host: GitHub's names allowed, others refused, with request counts and sizes." >}}
{{< /tab >}}
{{< tab name="In the menu bar" >}}
Chorus stays out of the way in the menu bar. The number beside its icon is the jobs running now.

{{< shot name="menu" class="narrow" width="300" height="208" alt="The Chorus menu-bar panel, listing the job VMs that are running." >}}
{{< /tab >}}
{{< /tabs >}}
