# lens-applier-cli

**Disposable, single-purpose infrastructure. Not the final architecture.**

This repository holds exactly one thing: a Release asset (`config-apply-cli.js`) — a standalone,
dependency-free esbuild bundle of `OkatiLtd/lens`'s configuration-applier CLI
(`extensions/lens-bridge/src/applier/cli.ts`).

## Why this exists

`OkatiLtd/lens` is private. The reconciler's R0 slice (`lens#246`) needs tenant-repo CI
(`OkatiLtd/strate-tenant-repo-test`, and any tenant repo after it) to run `config-apply plan`
without ever holding git/source access into a private Okati repo — the product operator ruled
that out explicitly, even for a read-only, single-repo-scoped credential such as a deploy key:

> "I don't want to give tenants access to a private repo. I want to give them a
> download/launcher for Lens."

This repo is the smallest thing that satisfies that: a genuinely public download, built from a
pinned `lens` commit, with zero credential of any kind needed to fetch it.

## What this is NOT

The operator's actual destination is this check running as a **plugin bundled into the Lens
download/launcher itself** — not a separately-fetched CLI, and not a mechanism worth hardening.
This repo, the release tag/asset scheme, and the tenant-repo CI step that downloads from it are
all interim scaffolding for R0. **Delete this repository once that plugin ships.** Do not add
versioning, changelogs, CI, or anything else here beyond what R0 needs — see `lens#246` for the
full history and reasoning.

## Source of truth

The bundle is built from `OkatiLtd/lens`'s `extensions/lens-bridge/esbuild.mjs` (`applierCli`
target). Each release here records, in its notes, the exact `lens` commit it was built from.
