---
title: Chorus
layout: hextra-home
---

<div class="chorus-hero">
<div>
{{< hextra/hero-headline >}}
  Your Mac, as a GitHub&nbsp;Actions runner
{{< /hextra/hero-headline >}}
<div class="hx:mt-6"></div>
{{< hextra/hero-subtitle >}}
  A fresh macOS VM for every Xcode job, built from a file in your
  repository, with its network filtered on the Mac you own.
{{< /hextra/hero-subtitle >}}
<div class="chorus-hero-actions">
{{< hextra/hero-button text="Get Started" link="docs/getting-started" >}}
<a class="chorus-secondary" href="https://github.com/bmorton/ChorusApp/releases/latest">Download for Apple silicon</a>
</div>
</div>
{{< shot name="overview" alt="The Chorus window: two job VMs, one running unit tests and one starting up, above a chart of what Chorus keeps on the Mac's disk." >}}
</div>

{{< hextra/feature-grid >}}
  {{< hextra/feature-card title="Getting Started" subtitle="From nothing to one job running in a Chorus VM." link="docs/getting-started" >}}
  {{< hextra/feature-card title="The Image" subtitle="Say which macOS, Xcode, simulators and tools a VM holds." link="docs/image" >}}
  {{< hextra/feature-card title="Network" subtitle="What a job may reach, and how to see what it asked for." link="docs/network" >}}
  {{< hextra/feature-card title="Caches" subtitle="Warm builds that only your default branch can update." link="docs/caches" >}}
  {{< hextra/feature-card title="More Repositories and Macs" subtitle="Serve several repositories, or share jobs between Macs." link="docs/more" >}}
  {{< hextra/feature-card title="Troubleshooting" subtitle="What to check, and how to update or remove Chorus." link="docs/troubleshooting" >}}
{{< /hextra/feature-grid >}}

<div class="chorus-section">
<h2>What it is like to use</h2>
<p class="chorus-lede">Chorus shows you what each job did, and lets you decide what it may do.</p>
</div>

<div class="chorus-shots">

{{< tabs >}}
{{< tab name="Where a job's time went" selected=true >}}
Every job gets a profile: how long each stage took, from clone to destroy, and whether the VM was short of CPU or memory.

{{< shot name="vm-succeeded-profile" alt="A finished job's profile: what stood out, a timeline from clone to destroy, and measurements of CPU, memory, disk and downloads." >}}
{{< /tab >}}
{{< tab name="What a job tried to reach" >}}
The Mac decides what a job may reach, and shows you every name it asked for and what was refused.

{{< shot name="vm-traffic-hosts" alt="A running job's traffic, by host: GitHub's names allowed, others refused, with request counts and sizes." >}}
{{< /tab >}}
{{< tab name="Settings, applied together" >}}
Each repository has a page: its image, its jobs of the last week, its cache. Change what its jobs may reach and how long they may run, then apply the changes together.

{{< shot name="repository-changes" height="952" alt="A repository's page: tiles for its image, jobs and connection, and a settings card with two changes waiting to be applied." >}}
{{< /tab >}}
{{< tab name="In the menu bar" >}}
Chorus stays out of the way in the menu bar. The number beside its icon is the jobs running now.

{{< shot name="menu" class="narrow" width="300" height="208" alt="The Chorus menu-bar panel, listing the job VMs that are running." >}}
{{< /tab >}}
{{< /tabs >}}

</div>
