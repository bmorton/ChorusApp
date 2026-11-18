---
title: Chorus
layout: hextra-home
---

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

{{< shot name="overview" alt="The Chorus window: two job VMs, one running unit tests and one starting up, above a chart of what Chorus keeps on the Mac's disk." >}}

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

## See where a job's time went

{{< shot name="vm-succeeded-profile" alt="A finished job's profile: what stood out, a timeline from clone to destroy, and measurements of CPU, memory, disk and downloads." caption="Every job gets a profile: how long each stage took, and whether the VM was short of CPU or memory." >}}

## See what a job tried to reach

{{< shot name="vm-traffic-hosts" alt="A running job's traffic, by host: GitHub's names allowed, others refused, with request counts and sizes." caption="The Mac decides what a job may reach, and shows you every name it asked for and what was refused." >}}

## Out of the way until you want it

{{< shot name="menu" class="narrow" alt="The Chorus menu-bar panel, listing the job VMs that are running." caption="Chorus lives in the menu bar. The number beside its icon is the jobs running now." >}}
