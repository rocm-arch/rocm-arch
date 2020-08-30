# ROCm for Arch Linux
This repository hosts a collection of [Arch Linux](https://www.archlinux.org/) [PKGBUILDs](https://wiki.archlinux.org/index.php/PKGBUILD) for the [AMD ROCm Platform](https://rocmdocs.amd.com/en/latest/).
These scripts implement a great portion of the stack, ranging from low level interfaces, over compilers to high-level application librariers.

## Installation
The Arch Linux packages for ROCm are available on the [AUR](https://wiki.archlinux.org/index.php/Arch_User_Repository).
Since many packages will be installed, we recommend to use the [`arch4edu`](https://wiki.archlinux.org/index.php/Unofficial_user_repositories#arch4edu) binary repository as it will
greatly speed up your installation time.
For directions see [Add arch4edu to your Archlinux](https://github.com/arch4edu/arch4edu/wiki/Add-arch4edu-to-your-Archlinux).

To install ROCm, use
```bash
# pacman -Syu rocm-dev rocm-utils rocm-libs
```

You can also install specific ROCm packages like so:
```bash
# pacman -S rocminfo
```

For additional installation configuration, such as adding a user to the `video`
group, we refer to AMD's [installation guide](https://rocmdocs.amd.com/en/latest/Installation_Guide/Installation-Guide.html).

To uninstall, use the following command:
```bash
# pacman -R rocm-dev rocm-utils rocm-libs
```

For more helpful tips see the ArchWiki entry on ROCm, [here](https://wiki.archlinux.org/index.php/GPGPU#ROCm).

## Contributing
Your contribution is always welcome. You can use the [issue tracker](https://github.com/rocm-arch/rocm-arch/issues) to report problems with the AUR packages.
Optimally, you open a pull request that solves your problem.
For most packages, you have to update `pkgver` and `sha256sums`. Before you commit your changes please generate `.SRCINFO` from the updated `PKGBUILD`:
```bash
makepkg --printsrcinfo > .SRCINFO
```
and add it to your commit.
As we want to bring ROCm into `community` we would greatly appreciate if you test that the package builds in a [clean chroot](https://wiki.archlinux.org/index.php/DeveloperWiki:Building_in_a_clean_chroot).
