# ROCm for Arch Linux
This repository hosts a collection of [Arch Linux](https://www.archlinux.org/) [PKGBUILDs](https://wiki.archlinux.org/index.php/PKGBUILD) for the [AMD ROCm Platform](https://rocmdocs.amd.com/en/latest/).
These scripts implement a great portion of the stack, ranging from low level interfaces, over compilers and high-level application librariers.

## Installation
The Arch Linux packages for ROCm are available on the [AUR](https://wiki.archlinux.org/index.php/Arch_User_Repository).
Since many packages will be installed, it is recommended to use an [AUR helper](https://wiki.archlinux.org/index.php/AUR_helpers)
like [`yay`](https://aur.archlinux.org/packages/yay/).

It is also recommended to use the [`arch4edu`](https://wiki.archlinux.org/index.php/Unofficial_user_repositories#arch4edu) binary repository as it will
greatly speed up your installation time.
For directions see [Add arch4edu to your Archlinux](https://github.com/arch4edu/arch4edu/wiki/Add-arch4edu-to-your-Archlinux).

To install ROCm, use the following command:
```
yay -S rocm-dkms
```
> **Warning**: Building the compiler `llvm-amdgpu` needs a lot of RAM (over 40 GiB on 12 threads) and disk space (more than 50 GiB). We recommend to use a [swap file](https://wiki.archlinux.org/index.php/swap#Swap_file).

You can also install specific ROCm packages like so:
```
yay -S rocminfo
```

For additional installation configuration, such as adding a user to the `video`
group, we refer to AMD's [installation guide](https://rocmdocs.amd.com/en/latest/Installation_Guide/Installation-Guide.html).

To uninstall, use the following command:
```
yay -R rocm-dkms 
```

## Status
Currently, we are updating the packages from `3.3.0` to the new `3.5.0` release. You can monitor the progress [here](https://github.com/rocm-arch/rocm-arch/milestones).

## Contributing
Any contribution is always welcome. You can use the [issue tracker](https://github.com/rocm-arch/rocm-arch/issues) to report problems with the AUR packages.
Optimally, you open a pull request that solves your problem.
If you want to help in packaging the new ROCm release, take a look at our [milestone](https://github.com/rocm-arch/rocm-arch/milestones) and pick an issue not yet assigned.
For most packages, you have to update `pkgver` and `sha256sums`. Before you commit your changes please generate `.SRCINFO` from the updated `PKGBUILD`:
```
makepkg --printsrcinfo > .SRCINFO
```
and add it to your commit.
As we want to bring ROCm into `community` we would greatly appreciate if you test that the package builds in a [clean chroot](https://wiki.archlinux.org/index.php/DeveloperWiki:Building_in_a_clean_chroot).

## FAQ

### How do I use TensorFlow with ROCm?
Unfortunately, this is not possible at the moment.
We first need to ensure that all dependencies (for instance `rocblas`) are fully functional.
But the more people contribute, the more likely is a working TensorFlow with ROCm on Arch Linux.
