---
title: Getting started
weight: 1
---

This takes a Mac from nothing to running one GitHub Actions job in a
Chorus VM. Most of the time is waiting: the first image build takes about
an hour.

Chorus is in beta. If a step here does not match what you see, the app is
right and this page is out of date; please say so in
[an issue](https://github.com/bmorton/ChorusApp/issues).

## What you need

- An Apple silicon Mac that stays on and logged in. Chorus runs in your
  login session, so jobs wait while the Mac is asleep or logged out.
- A recent macOS. `chorus doctor` and the app's setup page say whether
  yours is new enough.
- Memory for the VMs: 16 GB runs one job at a time, 32 GB runs two.
  macOS allows two macOS VMs at once, so two is the most.
- Free disk space. An image with one Xcode and two simulator runtimes
  takes tens of gigabytes; leave well over 100 GB free.
- [Homebrew](https://brew.sh), to install Tart and Softnet.
- A private GitHub repository you administer. Chorus does not serve
  public repositories, where anyone can open a pull request that runs
  on your Mac.
- An Xcode archive (`.xip`) downloaded from
  [Apple's developer site](https://developer.apple.com/download/all/).
  Apple requires your account for this, so Chorus cannot fetch it for you.

## 1. Install Tart and Softnet

```
brew install openai/tools/tart openai/tools/softnet
```

Tart runs the VMs. Softnet filters their network, and needs permission to
do that; the app shows the exact command in the next step.

## 2. Install Chorus

Download the latest `Chorus-….zip` from the
[Releases](https://github.com/bmorton/ChorusApp/releases) page, unzip it, move `Chorus.app` to
Applications and open it. Its window opens, and it appears in the Dock
and the menu bar.

The command-line tool is inside the app. To use it as `chorus` in
Terminal, open Chorus's Settings and click **Install Command-Line Tool**.
It links the tool into Homebrew's directory and needs no password.

## 3. Set up

Follow the setup page in the Chorus window.

1. **This Mac.** Anything missing is listed with what to do about it. For
   Softnet it gives a command to run in Terminal that asks for your Mac
   password once. Click **Check Again** when done.
2. **What jobs may reach.** Leave this on GitHub only unless your jobs
   need the rest of the internet.
3. **Create a GitHub App.** Chorus opens GitHub in your browser to create
   an App that belongs to you, then asks you to install it on one
   repository. It may register runners there and read the repository's
   code and workflow runs. Its key stays on this Mac.
4. **Start the daemon.** Click **Start Daemon at Login**. macOS may tell
   you a background item was added; that is the daemon.

The overview now shows the label your jobs will ask for, such as
`chorus-myrepo`.

## 4. Say what a VM should contain

In a checkout of your repository, create `.chorus/image.yaml`:

```yaml
version: 1

base:
  macos: "27"

xcode:
  - version: "27"
    platforms:
      - ios: "27"
    simulators:
      - iPhone 17 Pro

brew:
  - xcbeautify

caches:
  - xcode
  - swiftpm
```

Use the macOS, Xcode, simulator and device names your project builds
with.

Add your Xcode archive to this Mac's library, then pin the manifest to
exact versions:

```
chorus xcode add ~/Downloads/Xcode_27.xip
chorus image lock -C /path/to/your/checkout
```

`chorus image lock` writes `.chorus/image.lock` beside the manifest.
Commit both files and get them onto the repository's default branch.
Chorus reads them from there and nowhere else.

## 5. Build the image

Within a few minutes the app's overview says the repository's lock pins an
image that is not built.

To serve another repository from the same Mac, click **Add Repository…**
in the sidebar and give its address. If the GitHub App is not installed
on it yet, GitHub opens so that you can add it. The repositories share
the Mac's two VMs; each needs its own `.chorus/image.yaml` and lock, and
two repositories with the same files share one image.

To have a second Mac share the first one's jobs, set it up the same way,
then open the repository's page in its sidebar and enter the label your
workflows ask for (the first Mac's `runs-on` label) under **Sharing Jobs
with Other Macs**.
GitHub then gives each job to one of the Macs.

If this Mac's library lacks the Xcode the lock pins, as on a second Mac
that joins a repository already set up, **Needs You** says which one it
needs. **Download Xcode from Apple** opens Apple's download page, and
**Add Xcode Archive…** takes the `.xip` once it has downloaded.

Then click **Build Image**, or run:

```
chorus image build
```

The first build downloads macOS from Apple, installs it in a VM with
nobody typing, then adds Xcode, the simulators, your tools and the Actions
runner. Expect about an hour. Later builds reuse whatever has not changed,
so adding a tool takes minutes.

`chorus status` and the app show how far a build is.

{{< shot name="overview-image-building" alt="The overview while an image is being built, layer by layer." >}}

## 6. Run a job

Point a job at the label from step 3:

```yaml
jobs:
  test:
    runs-on: chorus-myrepo
    steps:
      - uses: actions/checkout@v4
      - run: xcodebuild test -scheme MyApp -destination 'platform=iOS Simulator,name=iPhone 17 Pro'
```

When the job is queued, Chorus starts a VM for it. The app shows the VM,
its log, the network names it asked for and where its time went.

{{< shot name="overview" alt="The Chorus window with two job VMs: one running a job, one starting up." >}}

## Things to know

- **Network.** By default a job can reach GitHub and nothing else. A step
  that downloads from anywhere else fails; put what the job needs in the
  image instead. The app's Traffic tab shows what a job asked for and what
  was refused.
- **Caches.** With `caches:` in the manifest, every job starts from the
  cache your default branch last produced. Only a successful push to the
  default branch updates it. Remove any other cache step from the job.
- **Changing the image.** Edit `image.yaml`, run `chorus image lock`
  again, merge both files, then build. Jobs keep using the old image until
  the new one is ready.
- **Time limit.** A VM is force-stopped after its time limit, which the
  overview shows.

## When something is wrong

```
chorus doctor       # what this Mac lacks
chorus status       # the daemon, its image and its VMs
chorus image list   # image layers and the last build
chorus cache list   # cache volumes, and why the last one was kept or not
```

The daemon's log is `~/Library/Logs/Chorus/chorusd.log`.

## Updating and removing

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

To remove Chorus: click **Stop Daemon** in Chorus's Settings, delete
`Chorus.app`, delete the VMs whose names start with `chorus-` (`tart list`
shows them, `tart delete <name>` removes one), delete
`~/Library/Application Support/Chorus`, and uninstall the GitHub App from
your repository's settings.
