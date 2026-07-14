## About this fork: `linux-cachyos-surface-latest`

This fork adds a **CachyOS kernel packaging** on top of upstream
linux-surface — see [`pkg/linux-cachyos-surface-latest/PKGBUILD`](pkg/linux-cachyos-surface-latest/PKGBUILD).
Unlike its sibling [`linux-cachyos-surface`](https://github.com/Steefzar/linux-cachyos-surface)
(which tracks whatever kernel version upstream linux-surface currently has
*official* patches for), this one tracks the **newest CachyOS kernel
release**, even ahead of upstream support:

- If linux-surface's newest tag already covers that kernel's major version,
  the build uses their patches directly — identical to the sibling package.
- If not, it falls back to a hand-rebased patch set bundled in this repo
  ([`rebased.patch`](pkg/linux-cachyos-surface-latest/rebased.patch), see
  [`STATUS`](pkg/linux-cachyos-surface-latest/STATUS) for what it was last
  validated against). If that no longer applies against a newer kernel, the
  build aborts with a clear message instead of shipping something broken.

> **Prefer only kernel versions linux-surface officially supports?** Use
> the sibling package:
> [`linux-cachyos-surface`](https://github.com/Steefzar/linux-cachyos-surface).

**What it's for:** running the latest CachyOS kernel (EEVDF/BORE scheduler,
LTO, BBR3, CachyOS's other tuning) with Surface hardware support, without
waiting for upstream linux-surface to catch up to each new CachyOS release.

**How to use it:**

*Precompiled packages (easiest — no local kernel build):* builds of this
package (and its sibling) are published as a signed pacman repository served
from GitHub Releases. One-time setup — first trust the signing key:

```sh
curl -s https://raw.githubusercontent.com/Steefzar/linux-cachyos-surface-latest/master/pkg/keys/surface-cachyos.asc \
    | sudo pacman-key --add -
sudo pacman-key --finger F43A86B2BAA715242965C241222C2A1B58F14BA7
sudo pacman-key --lsign-key F43A86B2BAA715242965C241222C2A1B58F14BA7
```

Then add the repository to `/etc/pacman.conf`:

```ini
[surface-cachyos]
Server = https://github.com/Steefzar/surface-kernel-autoupdate/releases/download/repo
```

And install:

```sh
sudo pacman -Syu linux-cachyos-surface-latest linux-cachyos-surface-latest-headers
```

*Build it yourself, auto-updated:* use the installer at
[Steefzar/surface-kernel-autoupdate](https://github.com/Steefzar/surface-kernel-autoupdate),
which sets up a local pacman repo and auto-builds/updates this package (and
its sibling `linux-cachyos-surface`) as part of your `yay` runs.

*Fully manual:* `makepkg` inside `pkg/linux-cachyos-surface-latest/` like any
PKGBUILD.

This package builds on two upstreams: the [CachyOS
kernel](https://github.com/CachyOS/linux) provides the base (its patches ship
pre-applied in the source tarball), and linux-surface (below) provides the
Surface hardware patches applied on top — hand-rebased here whenever
linux-surface hasn't caught up to the newest CachyOS release yet.

### Credits

- [linux-surface/linux-surface](https://github.com/linux-surface/linux-surface)
  (GPL-2.0) — every Surface hardware-enablement patch this kernel carries,
  whether applied directly or as the basis of the hand-rebase, originates
  there. This repo's git history is seeded from theirs.
- [CachyOS/linux](https://github.com/CachyOS/linux) and
  [CachyOS/linux-cachyos](https://github.com/CachyOS/linux-cachyos) — the
  CachyOS kernel base (EEVDF/BORE scheduler, BBR3, LTO and other CachyOS
  tuning) this build starts from, and the packaging conventions this
  PKGBUILD is adapted from.

This packaging, and the hand-rebase of linux-surface's patches bundled as
`rebased.patch`, were produced with Claude Code (Anthropic) assistance, at
the direction of and reviewed/tested (built, installed, booted, confirmed
working) by the repository owner. Commits made with Claude's help are marked
`Co-Authored-By: Claude <noreply@anthropic.com>`.

---

## About upstream: linux-surface

Everything outside `pkg/` in this repository is the work of
[linux-surface/linux-surface](https://github.com/linux-surface/linux-surface),
the project that maintains the kernel patches making Surface hardware work on
Linux (the Surface Aggregator Module, keyboard/touchpad, touch/pen input,
cameras, and more) — this repo is a fork of theirs with packaging added on
top.

- **Is your device supported?** See their
  [supported devices & feature matrix](https://github.com/linux-surface/linux-surface/wiki/Supported-Devices-and-Features#feature-matrix).
- **Hardware tips** (disk encryption, hibernation, TLP caveats, extra tweaks
  in `contrib/`): the [linux-surface wiki](https://github.com/linux-surface/linux-surface/wiki).
- **Support:** problems with *this package or its builds* — open an issue
  here. Hardware and patch questions — upstream's
  [Matrix space](https://matrix.to/#/#linux-surface:matrix.org); please don't
  report issues from this unofficial build there unless you can reproduce
  them on an official linux-surface kernel.

## License

This repository contains patches, which are either derivative work targeting a specific already licensed source, i.e. parts of the Linux kernel, or introduce new parts to the Linux kernel.
These patches fall thus, if not explicitly stated otherwise, under the license of the source they are targeting, or if they introduce new code, the license they explicitly specify inside of the patch.
Please refer to the specific patch and source in question for further information.
License texts can be obtained at https://github.com/torvalds/linux/tree/master/LICENSES.
