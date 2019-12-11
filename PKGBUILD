# Maintainer: Ranieri Althoff <ranisalt+aur at gmail dot com>
_opencl_icd_loader_commit="978b4b3a29a3aebc86ce9315d5c5963e88722d03"

pkgname=rocm-opencl-runtime
pkgver=2.10.0
pkgrel=2
pkgdesc='ROCm OpenCL™ Compatible Runtime'
arch=('x86_64')
url='https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime'
license=('MIT')
depends=("rocr-runtime>=${pkgver}" 'opencl-icd-loader')
makedepends=(mesa cmake ninja git)
provides=("${pkgname}" 'opencl-driver')
conflicts=('rocm-opencl-git')
source=(
    "rocm-opencl-runtime::git+https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime#tag=roc-${pkgver}"
    "rocm-opencl-driver::git+https://github.com/RadeonOpenCompute/ROCm-OpenCL-Driver#tag=roc-${pkgver}"
    "llvm-ocl-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/llvm/archive/roc-ocl-${pkgver}.tar.gz"
    "clang-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/clang/archive/roc-${pkgver}.tar.gz"
    "lld-ocl-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/lld/archive/roc-ocl-${pkgver}.tar.gz"
    "rocm-device-libs::git+https://github.com/RadeonOpenCompute/ROCm-Device-Libs#tag=roc-ocl-${pkgver}"
    "opencl-icd-loader::git+https://github.com/KhronosGroup/OpenCL-ICD-Loader#commit=${_opencl_icd_loader_commit}"
    'fix_rocm_opencl_build_order.patch'
)

sha256sums=('SKIP'
            'SKIP'
            'c46ec3568b754a7aced6139bcc254d39ddb89016b282c467ae1b5536d8021540'
            'acdebf229a2120ad0ccdc48a6b0e863a6a4d89f46bb62e7296f2f16bbc4a04a7'
            '105cedd4a31a6d6ad51043c384129a7705f9a5f65414e60a07c9193411b756f0'
            'SKIP'
            'SKIP'
            '0f735299810e9e22cd57f5b96a7628a0f399d445a1f3bacefd8b890e84ca372f')

prepare() {
    # emulate repo opencl.xml layout
    cd "$srcdir"

    ln -s "$srcdir/rocm-opencl-runtime" opencl

    mkdir -p "$srcdir/opencl/compiler"
    ln -s "$srcdir/rocm-opencl-driver" opencl/compiler/driver
    ln -s "$srcdir/llvm-roc-ocl-$pkgver" opencl/compiler/llvm

    mkdir -p "$srcdir/opencl/compiler/llvm/tools"
    ln -s "$srcdir/clang-roc-$pkgver" opencl/compiler/llvm/tools/clang
    ln -s "$srcdir/lld-roc-ocl-$pkgver" opencl/compiler/llvm/tools/lld

    mkdir -p "$srcdir/opencl/library"
    ln -s "$srcdir/rocm-device-libs" opencl/library/amdgcn

    mkdir -p "$srcdir/opencl/api/opencl/khronos"
    ln -s "$srcdir/opencl-icd-loader" opencl/api/opencl/khronos/icd

    # Fix build missing dependency
    # https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime/issues/64#issuecomment-473502685
    cd "$srcdir/opencl"
    patch -Np1 -i "${srcdir}/fix_rocm_opencl_build_order.patch"
}

build() {
    mkdir -p "$srcdir/build"
    cd "$srcdir/build"

    if check_buildoption "ccache" "y"; then
        CMAKE_FLAGS="-DROCM_CCACHE_BUILD=ON"
    fi

    cmake $CMAKE_FLAGS \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/opt/rocm/opencl \
        -DCLANG_ENABLE_STATIC_ANALYZER=ON \
        -G Ninja \
        "$srcdir/opencl"

    # building LLVM/Clang requires ~1.5G per unit
    THREADS=$(( ($(getconf _PHYS_PAGES) * $(getconf PAGESIZE)) / 1610612736 ))
    if [ "$THREADS" -lt 1 ]; then
        THREADS=1
    fi
    ninja "-j$THREADS"

    # FIXME Check why lld has to built manually since 2.3.0
    ninja lld
}

package() {
    DESTDIR="$pkgdir/" ninja -C "$srcdir/build" install

    # OpenCL vendor file
    mkdir -p "$pkgdir/etc/OpenCL/vendors"
    echo "/opt/rocm/opencl/lib/x86_64/libamdocl64.so" > "$pkgdir/etc/OpenCL/vendors/rocm-opencl64.icd"

    mkdir -p "$pkgdir/etc/ld.so.conf.d"
    echo "/opt/rocm/opencl/lib" > "$pkgdir/etc/ld.so.conf.d/rocm-opencl.conf"

    # Discard AMD's OpenCL ICD Loader
    # https://wiki.archlinux.org/index.php/GPGPU#OpenCL_ICD_loader_(libOpenCL.so)
    rm -f "$pkgdir"/opt/rocm/opencl/lib/x86_64/libOpenCL.so*
}
