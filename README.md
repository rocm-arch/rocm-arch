# ROCm for Arch Linux AUR
This repository hosts a collection of [Arch Linux](https://www.archlinux.org/)
[PKGBUILDs](https://wiki.archlinux.org/index.php/PKGBUILD) for
[AMD ROCm Software](https://www.amd.com/en/products/software/rocm.html) that have
yet to be added to the official Arch Linux repositories.
All important ROCm packages are in the official Arch Linux repositories.

## Installing from the AUR/Building from source

You can install any package listed here from the AUR.
An example using the `paru` AUR helper.

```bash
paru -S <package-name>
```

### Common issues

Please consult this list first before opening an issue.

#### Package X does not work
Please first check if your GPU is supported by ROCm.
A complete list is found in the [official documentation](https://rocm.docs.amd.com/projects/install-on-linux/en/latest/reference/system-requirements.html)

#### Package X does not build on OS Y
We only support Arch Linux. If a package does not build, first consider building in a
[clean chroot](https://wiki.archlinux.org/index.php/DeveloperWiki:Building_in_a_clean_chroot).
The AUR helper `paru` supports building in clean chroot. Call

```bash
paru --chroot
```

and follow the instructions. Afterward, you can build packages in a clean chroot by calling

```
paru --chroot -S <package-name>
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
Use the issues page of the problematic package in the [Arch Linux GitLab](https://gitlab.archlinux.org/archlinux/packaging/packages) to report problems for the offical packages.
