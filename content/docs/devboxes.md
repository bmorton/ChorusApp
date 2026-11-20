---
title: Devboxes
weight: 5
---

A devbox is a VM of your own to work in: the same macOS, Xcode and tools
a job gets, behind the same network rules, but kept until you delete it.
It is made for running a coding agent such as Claude Code somewhere it
can build your app and cannot reach your Mac, your network or your
other work.

{{< shot name="devbox" alt="A devbox's page in Chorus: running, holding one of two VMs, in Restricted mode, with the command that opens a terminal in it." >}}

## Make one and open a terminal

In the app, the **+** at the head of the sidebar has **New Devbox…**.
Name it, choose the repository whose image to clone, tick what it may
reach, and create it. Its page has **Open Terminal**, which starts it and
opens Terminal on it.

{{< shot name="new-devbox" class="narrow" width="560" height="445" alt="The New Devbox sheet: a name, a repository, Restricted or Observe with groups to tick, and when to suspend it." >}}

The same from the command line:

```sh
chorus box create agent
chorus box shell agent
```

`create` clones a repository's image: the first repository this Mac
serves, or the one you name with `-profile <runs-on name>`. `shell`
starts the box and gives you a terminal in it. Sign in to your agent and
to GitHub there, as you would on a new Mac.

The terminal is an ordinary login shell, and it ends when you close
the window, taking what was running in it along. For work that should
carry on with no window open, such as an agent left to finish a task,
run a terminal multiplexer such as tmux inside the box and start the
work there; `chorus box shell agent` and then `tmux attach` brings you
back to it. macOS has no tmux of its own, so it has to be in the
repository's image or installed in the box. Suspending a box keeps
everything in it, a tmux session included.

## What a devbox can reach

The same two modes a repository has, enforced the same way and written
to the same record; the [network page](../network) describes them. A
new box takes its repository's mode, that repository's groups with
**Claude Code** added, and the names you allowed there. To choose
otherwise:

```sh
chorus box create -egress restricted -groups claude-code -allow registry.npmjs.org agent
```

`create` says what the box was given. It is set when the box is made;
to change it, make another.

In every mode a devbox cannot reach this Mac, your local network or a
job's VM, and nothing can connect to it. You reach it through Chorus on
the Mac, never over the network.

## Chorus puts no secret in it

A job is handed one single-use registration and nothing else. A devbox
is handed nothing at all. Whatever you sign in to inside it is on its
disk, stays there while it lives, and is deleted with it. An agent
working in the box can use those sign-ins, so the network mode is what
bounds where it can send them: give a box only what its work needs.

## Suspend, shut down, delete

```sh
chorus box list             # your devboxes and where each stands
chorus box suspend agent    # put it away with everything still running
chorus box start agent      # carry it on from where it was
chorus box stop agent       # shut it down; its disk is kept
chorus box delete agent     # delete it, with everything in it
```

Suspending takes about half a minute and saves the box's memory with its
disk. An agent in the middle of something is in the middle of it again
when you start the box.

A box that sits idle is suspended for you after two hours, so that a
forgotten one does not keep a VM from your jobs. Idle means no terminal
open on it through `chorus box shell` and a quiet processor. Set another
time when you make it, or none:

```sh
chorus box create -idle 8h agent
chorus box create -idle never agent
```

When Chorus restarts, for an update or otherwise, a running box is
suspended first and carried on afterwards. If the Mac itself restarts or
loses power, the box is shut down: its disk is kept, and what was
running in it is not.

## One of your two VMs

Apple allows two macOS VMs on a Mac. A running devbox is one of them, so
while it runs this Mac takes one job at a time. A suspended or shut down
box takes neither.

- One devbox runs at a time. You can keep up to eight.
- A devbox is never stopped to make room for a job, and it starts only
  when a VM is free.
- A devbox is never given a job, and a job's VM never becomes a devbox.

## Commands and files

```sh
chorus box exec agent xcodebuild -version      # run one command
chorus box cp notes.md agent:work/notes.md     # copy a file in
chorus box cp agent:work/build.log .           # and one out
```

A path in the box is absolute, or starts from its home folder. One file
is copied at a time. Treat what comes out of a box as you would a
download: it is whatever the box put there.

## Not yet

- SSH, for editors that connect that way.
- A window onto the box's screen.
- A time after which a box deletes itself.
- Your repository's cache volumes inside a devbox.
