# Maintainer: Ulysses R Ribeiro <ulyssesrr@gmail.com>
_opencl_icd_loader_commit="978b4b3a29a3aebc86ce9315d5c5963e88722d03"

pkgname=rocm-opencl-runtime
pkgver=2.10.0
pkgrel=1
pkgdesc='ROCm OpenCL™ Compatible Runtime'
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime"
license=('MIT')
depends=("rocr-runtime>=${pkgver}" "opencl-icd-loader")
makedepends=(mesa cmake ninja git)
provides=("${pkgname}" "opencl-driver")
conflicts=("rocm-opencl-git")
source=(
    "rocm-opencl-runtime-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime/archive/roc-${pkgver}.tar.gz"
    "rocm-opencl-driver-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/ROCm-OpenCL-Driver/archive/roc-${pkgver}.tar.gz"
    "llvm-roc-ocl-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/llvm/archive/roc-ocl-${pkgver}.tar.gz"
    "clang-roc-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/clang/archive/roc-${pkgver}.tar.gz"
    "lld-roc-ocl-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/lld/archive/roc-ocl-${pkgver}.tar.gz"
    "rocm-device-libs-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/ROCm-Device-Libs/archive/roc-ocl-${pkgver}.tar.gz"
    "OpenCL-ICD-Loader-${_opencl_icd_loader_commit}.zip::https://github.com/KhronosGroup/OpenCL-ICD-Loader/archive/${_opencl_icd_loader_commit}.zip"
    'fix_rocm_opencl_build_order.patch'
)

sha256sums=('7b38b1be2217d7b0ac552e4cfb5fe38660fa59ef61f2a46946bac6fba6efdf7c'
            '00a50696732a4cd3b880cfb11a64d02c5dfd956a79796eacda166173c14eb97a'
            'c46ec3568b754a7aced6139bcc254d39ddb89016b282c467ae1b5536d8021540'
            'acdebf229a2120ad0ccdc48a6b0e863a6a4d89f46bb62e7296f2f16bbc4a04a7'
            '105cedd4a31a6d6ad51043c384129a7705f9a5f65414e60a07c9193411b756f0'
            'ed56c5f742bd8b2fc7e1124434fa575f51573900444adc235ce6785f3eb56d9d'
            '6c79fddd59e92182d7bc9048415b51e60e49f952b11eeffd69cccb3081359f78'
            '0f735299810e9e22cd57f5b96a7628a0f399d445a1f3bacefd8b890e84ca372f')

prepare() {
    # emulate repo opencl.xml layout
    cd "$srcdir"

    ln -s "$srcdir/ROCm-OpenCL-Runtime-roc-$pkgver" opencl

    mkdir -p "$srcdir/opencl/compiler"
    ln -s "$srcdir/ROCm-OpenCL-Driver-roc-$pkgver" opencl/compiler/driver
    ln -s "$srcdir/llvm-roc-ocl-$pkgver" opencl/compiler/llvm

    mkdir -p "$srcdir/opencl/compiler/llvm/tools"
    ln -s "$srcdir/clang-roc-$pkgver" opencl/compiler/llvm/tools/clang
    ln -s "$srcdir/lld-roc-ocl-$pkgver" opencl/compiler/llvm/tools/lld

    mkdir -p "$srcdir/opencl/library"
    ln -s "$srcdir/ROCm-Device-Libs-roc-ocl-$pkgver" opencl/library/amdgcn

    mkdir -p "$srcdir/opencl/api/opencl/khronos"
    ln -s "$srcdir/OpenCL-ICD-Loader-$_opencl_icd_loader_commit" opencl/api/opencl/khronos/icd

    # Fix build missing dependency
    # https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime/issues/64#issuecomment-473502685
    cd "$srcdir/opencl"
    patch -Np1 -i "${srcdir}/fix_rocm_opencl_build_order.patch"
}

build() {
    MEM_AVAIL=`cat /proc/meminfo | grep MemTotal | awk {'print $2'}`
    AVAIL_THREADS=`nproc`
    # require ~1536MB per thread
    MAX_THREADS=`echo $(( "$MEM_AVAIL" / $(( 1024 * 1536 ))))`
    if [ "$MAX_THREADS" -lt "$AVAIL_THREADS" ]; then
        NUM_BUILD_THREADS="$MAX_THREADS"
    else
        NUM_BUILD_THREADS="$AVAIL_THREADS"
    fi
    if [ "$NUM_BUILD_THREADS" -lt 1 ]; then
        NUM_BUILD_THREADS=1
    fi

    mkdir -p "$srcdir/build"
    cd "$srcdir/build"

    ## mimic AMD official binaries LDFLAGS
    #export LDFLAGS="-Wl,--no-as-needed"

    cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/opt/rocm/opencl \
        -DCLANG_ENABLE_STATIC_ANALYZER=ON \
        -G Ninja \
        "$srcdir/opencl"
    # -jNUM_BUILD_THREADS to avoid Out of Memory
    ninja "-j$NUM_BUILD_THREADS"

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
