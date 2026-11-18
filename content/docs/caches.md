---
title: Caches
weight: 4
---

A fresh VM for every job would mean compiling everything every time.
Cache volumes avoid that without letting one job leave something behind
for the next.

List them in `.chorus/image.yaml`:

```yaml
caches:
  - xcode      # DerivedData
  - swiftpm    # resolved Swift packages
```

A cache can also name its own folders, under the runner's home folder:

```yaml
caches:
  - name: go
    paths:
      - ~/Library/Caches/go-build
      - ~/go/pkg/mod
```

## How a cache is kept safe

- Every job starts from a copy of the cache your default branch last
  produced.
- Only a successful push to the default branch replaces it. Chorus asks
  GitHub whether the run was one; it does not take the job's word.
- What any other job writes to its copy, including a pull request's, is
  thrown away with the VM.

So a pull request builds quickly and cannot change what later jobs start
from.

## In your workflow

Remove any other cache step, such as `actions/cache`, from jobs that run
on Chorus: the volume is already there when the job starts.

`chorus cache list` shows each cache volume and why the last one was
kept or thrown away.
