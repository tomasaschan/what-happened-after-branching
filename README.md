# What happened before merge?

The purpose of this repo is to explore this problem: _What changes were made to a specific branch,
that has now been merged into `master`?_

## Background

The history of this repository looks like this:

```
X0 - X1 - - X - X - Xn - ... - Xinf
 \         /   /   /
  \   C - D   /   /
   \ /     \ /   /
    A - E - F - H
     \ /     \ /
      B       G
```

The `master` branch points to `Xinf`, and the `release` branch points to `H`.

After creating the `release` branch, a number of bugs were fixed, but they were handled differently:

- Some were contributed as PRs to the `release` branch, e.g. `B`.
- Some were contributed to `master`, and then cherry-picked to a PR to the `release` branch, e.g. `G`.
  Note that in this repo, where all commits are empty, `B` and `G` look exactly the same.
- Some were contributed as PRs that were merged into both `master` and `release`, e.g. `C-D`.

During this process, `release` was also merged back into `master` several times.

In this example I've ensured that all of these things have happened, but in reality they can have
happened more times (or less) than indicated by my example - and, obviously, also in different orders.

Eventually, the release candidate stabilized enough that it was released, and no more work was done
on the `release` branch.

## Problem

Now, we want to know this: **From the point that the `release` branch was created, to the point when
this version was actually released, which commits were added to the `release` branch?**

The desired output is the list of sha's for commits `A` through `H`; in this repo, that is

```
944c4fb
ce1a434
e5a00cc
54027ce
dc7c3ee
a5ab491
728a47c
f29d8dc
```

in no particular order.

## Solution Requirements

The solution to this problem is a `git` command or a script that, given the name of the `release`
branch and run in a repo, can list the SHAs of the relevant commits.

It must be reasonaby performant, so as to compleate in a sensible time even in large repositories
with tens of thousands of files and hundreds of thoudsands of commits.
