# SDDM-1779-PKGBUILD

If you use SDDM as your display manager and use fish as your default shell, you may facing the issue that SDDM failed to start your session.

On Aug 5, 2023, Vogtinator opened [a pull request](https://github.com/sddm/sddm/pull/1779) on GitHub. But it seems that the PR is not merged yet on Jan 14, 2025.

As an Arch Linux user, I forked the PKGBUILD from Arch Linux's repository to build the patched version of SDDM. You can find the PKGBUILD in this repository.

## How to build and install

```bash
git clone https://github.com/BeiyanYunyi/sddm-1779-PKGBUILD
cd sddm-1779-PKGBUILD
makepkg
sudo pacman -U ./sddm-BeiyanYunyi-0.21.0-6-x86_64.pkg.tar.zst
```

This package was marked as conflicting with the official `sddm` package and providing `sddm`. You may be asked to remove the official package before installing this one.

After replacing the official package with this one, your sddm should work with fish shell now.

## Maintainance

This repository will often be rebased to keep with the upstream, and will be archived after the PR is merged and the official package is updated.
