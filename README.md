# nextbsd-freebsd-compat

The **fbsdglue** set: the irreducibly-FreeBSD-only `/usr/src` userland that the
NextBSD live ISO must build *from source* (not from pkgbase packages) — boot-
critical, kernel-bound, or FreeBSD-specific tools with no Apple analogue.

Examples: `kldload`/`kldstat`/`kldunload`/`kldconfig`, UFS family
(`fsck_ffs`, `newfs`, `tunefs`, `mount`/`umount`), `devfs`, `ldconfig`, `ldd`,
`devctl`, `diskinfo`, `gstat`, `pciconf`, `pw`, `nologin`, `reboot`/`shutdown`,
`bin/sh`, `crashinfo`, `save-entropy`. (See the migrated source list once it
lands here.)

## Why a separate repo

Today these are rebuilt on every ISO build by `build.sh` (step 3a2) even when
they haven't changed — wasteful. Splitting them out lets them **build once →
publish an artifact** that the ISO build consumes, on their own trigger
(upstream source change or an explicit bump), independent of Apple-port churn
in `nextbsd-redux/nextbsd`.

## Status

**Scaffold only.** Content migrates from `nextbsd-redux/nextbsd`
(`srclist-fbsdglue.txt` + the `build.sh` fbsdglue step) in a later change.

## Build note (for the migration)

Unlike the kernel pipeline, these are userland `bin`/`sbin` targets, so the
build needs the **world** build bootstrap (buildworld libs), not just
`make kernel-toolchain`. Plan for a world-toolchain image (a heavier variant of
[`nextbsd-kernel-toolchain`](https://github.com/nextbsd-redux/nextbsd-kernel-toolchain),
or a sibling repo) that runs `make toolchain` + the needed bootstrap libs, then
builds each listed dir via `./tools/build/make.py`.

## Related

- Source build plan: https://pkgdemon.github.io/freebsd-srclist-build-plan.html
- Consumer: `nextbsd-redux/nextbsd` (ISO build)
