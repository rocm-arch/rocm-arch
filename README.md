# ROCm for Arch Linux
This repository hosts a collection of [Arch Linux](https://www.archlinux.org/)
[PKGBUILDs](https://wiki.archlinux.org/index.php/PKGBUILD) for the
[AMD ROCm Platform](https://www.amd.com/en/graphics/servers-solutions-rocm).
These scripts implement a great portion of the stack, ranging from low-level
interfaces, over compilers to high-level application libraries.

## Installation
The Arch Linux packages for ROCm are available on the
[AUR](https://wiki.archlinux.org/index.php/Arch_User_Repository).
Since many packages will be installed, it is recommended to use an
[AUR helper](https://wiki.archlinux.org/index.php/AUR_helpers)
like [`paru`](https://aur.archlinux.org/packages/paru/).

It is also recommended to use the
[`arch4edu`](https://wiki.archlinux.org/index.php/Unofficial_user_repositories#arch4edu)
binary repository as it will greatly speed up your installation time.
For directions see [Add arch4edu to your Archlinux](https://github.com/arch4edu/arch4edu/wiki/Add-arch4edu-to-your-Archlinux).

To install ROCm, use
```bash
paru -S rocm-hip-sdk rocm-opencl-sdk
```
which includes the low-level components and compilers, utilities like `rocminfo`
and GPU-accelerated math libraries. See the
[documentation](https://docs.amd.com/bundle/ROCm-Installation-Guide-v5.0.2/page/Meta-packages_in_ROCm_Programming_Models.html)
for an overview on the available meta packages.
> **WARNING** It is strongly recommended to remove all ROCm components when updating to a new release.
> Otherwise, building the packages with `paru` may have unwanted side effects resulting in build errors.
> If it's a small release with only a few updated packages, consider building them in a [clean chroot](https://wiki.archlinux.org/index.php/DeveloperWiki:Building_in_a_clean_chroot).
>
> One way to mitigate these issues is to use the binary versions of the packages provided by arch4edu.

## Building from source

ROCm stack comprises around 50 packages including a fork of LLVM.
Therefore, building all packages from source can take a long time and can use a lot of RAM.

### Common issues

Please consult this list first before opening an issue

#### Package X does not build on OS Y
We only support Arch Linux. If a package does not build, first consider building in a
[clean chroot](https://wiki.archlinux.org/index.php/DeveloperWiki:Building_in_a_clean_chroot).
The AUR wrapper `paru` has recently added support for building in clean chroot. Call
```bash
paru --chroot
```
and follow the instructions. Afterward, you can build packages in a clean chroot by calling
```
paru --chroot -S <PACKAGE NAME>
```

#### `hipcc` errors with stack protector enabled
Some commonly used compiler flags are unsupported by `clang` (and thus `hipcc`) from `rocm-llvm`,
including stack protection,
```bash
-fstack-protector-all
-fstack-protector-strong
-fstack-protector
```
See the [official documentation](https://docs.amd.com/bundle/ROCm-Compiler-Reference-Guide-v5.2/page/Appendix_A.html) for a full list.

For additional installation configuration, such as adding a user to the `video`
group, we refer to AMD's
[installation guide](https://docs.amd.com/bundle/ROCm-Installation-Guide-v5.2/page/Prerequisite_Actions.html).

To uninstall, use the following command:
```bash
paru -R rocm-hip-sdk rocm-opencl-sdk
```

### OpenCL runtime for Polaris
To enable OpenCL support on Polaris GPUs (gfx800s) set
```
ROC_ENABLE_PRE_VEGA=1
```
as an environment variable.

For more helpful tips see the ArchWiki entry on ROCm,
[here](https://wiki.archlinux.org/index.php/GPGPU#ROCm).

## Discussions/Support/Issues
For general concerns/comments/support we use
[Discussions](https://github.com/rocm-arch/rocm-arch/discussions).
If you have issues specific to your setup or are having difficulties
getting something to work, feel free to post there.

Use the [issue tracker](https://github.com/rocm-arch/rocm-arch/issues) to report
problems with the AUR packages.

## Contributing
Your contribution is always welcome. Before making a pull request, please open
an issue at the [issue tracker](https://github.com/rocm-arch/rocm-arch/issues)
to report the problem with build/error logs.
For most packages, you have to update `pkgver` and `sha256sums`. Before you commit your changes you will need to generate `.SRCINFO` from the updated `PKGBUILD`:
```bash
makepkg --printsrcinfo > .SRCINFO
```
and add it to your commit.
As we want to bring ROCm into `community` we would greatly appreciate if you test that the package builds in a [clean chroot](https://wiki.archlinux.org/index.php/DeveloperWiki:Building_in_a_clean_chroot).
