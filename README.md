# UltimMC package for Arch Linux

## How to install

You need to set up your Arch install to be able to build packages.

See instructions for that: https://wiki.archlinux.org/title/Makepkg#Usage

### Using git
If you have `git` installed, you can just clone the whole repository.

```
git clone https://github.com/UltimMC/ultimmc-pkgbuild.git
cd ultimmc-pkgbuild
makepkg -si
```

### Using wget
If you do not have `git` installed, you can use a downloader like `wget` or directly download the `PKGBUILD` using your browser.

```
mkdir ultimmc-pkgbuild
cd ultimmc-pkgbuild
wget https://raw.githubusercontent.com/UltimMC/ultimmc-pkgbuild/master/PKGBUILD
makepkg -si
```

## Issues?

Report issues [here](https://github.com/UltimMC/ultimmc-pkgbuild/issues).
