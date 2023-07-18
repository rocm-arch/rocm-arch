# ROCm for Arch Linux
This repository hosts a collection of [Arch Linux](https://www.archlinux.org/)
[PKGBUILDs](https://wiki.archlinux.org/index.php/PKGBUILD) for the
[AMD ROCm Platform](https://www.amd.com/en/graphics/servers-solutions-rocm).
A great portion of the stack is part of the official `[extra]` repository,
including all packages needed for Machine Learning.

## Installation
To install ROCm, use
```bash
pacman -S rocm-hip-sdk rocm-opencl-sdk
```
which includes the low-level components and compilers, utilities like `rocminfo`
and GPU-accelerated math libraries. See the
[documentation](https://rocm.docs.amd.com/en/latest/deploy/linux/os-native/package_manager_integration.html#components-of-rocm-programming-models)
for an overview on the available meta packages.

## Building from source

All important ROCm packages are in the official Arch Linux repositories.
Here, we disucss how to build the few remaining packages in the AUR.

### Common issues

Please consult this list first before opening an issue.

#### Package X does not work
Please first check if your GPU is supported by ROCm.
A complete list is found in the [official documentation](https://community.amd.com/t5/knowledge-base/amd-rocm-hardware-and-software-support-document/ta-p/489937)

#### Package X does not build on OS Y
We only support Arch Linux. If a package does not build, first consider building in a
[clean chroot](https://wiki.archlinux.org/index.php/DeveloperWiki:Building_in_a_clean_chroot).
The AUR wrapper `paru` supports building in clean chroot. Call
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
See the [official documentation](https://rocm.docs.amd.com/en/latest/reference/rocmcc/rocmcc.html#support-status-of-other-clang-options) for a full list.

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
Use the official [bugtracker](https://bugs.archlinux.org/) for the offical binary packages.
