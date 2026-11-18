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
  Chorus runs each Xcode job in a fresh macOS VM on a Mac you own,&nbsp;<br class="hx:sm:block hx:hidden" />
  built from a file in your repository, with its network filtered on the host.
{{< /hextra/hero-subtitle >}}
</div>

<div class="hx:mb-6">
{{< hextra/hero-button text="Get Started" link="docs/getting-started" >}}
</div>

<div class="hx:mt-6"></div>

{{< hextra/feature-grid >}}
  {{< hextra/feature-card title="A fresh VM for every job" subtitle="Each job gets a new clone of your image, which is stopped and deleted when the job ends. Nothing one job leaves behind reaches the next." >}}
  {{< hextra/feature-card title="The image is a file in your repository" subtitle="Say which macOS, Xcode, simulators and tools a VM holds in .chorus/image.yaml. Chorus builds the image and rebuilds only what changed." >}}
  {{< hextra/feature-card title="Network filtered on the host" subtitle="By default a job reaches GitHub and nothing else: not your Mac, not your network. The app shows what each job asked for and what was refused." >}}
  {{< hextra/feature-card title="Warm caches, kept safe" subtitle="Jobs start from the build cache your default branch last produced. Only a successful push to the default branch can update it." >}}
  {{< hextra/feature-card title="Two jobs at once, more Macs for more" subtitle="A Mac runs up to two macOS VMs, the most macOS allows. A second Mac can share the same runs-on label." >}}
  {{< hextra/feature-card title="In beta" subtitle="Chorus serves private repositories you administer, on Apple silicon. Expect rough edges, and please report them." link="https://github.com/bmorton/ChorusApp/issues" >}}
{{< /hextra/feature-grid >}}
