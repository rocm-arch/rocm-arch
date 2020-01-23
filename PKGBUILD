# Maintainer: Ranieri Althoff <ranisalt+aur at gmail dot com>

_opencl_icd_loader_commit='978b4b3a29a3aebc86ce9315d5c5963e88722d03'

pkgbase=rocm-opencl-runtime
pkgname=(rocm-device-libs rocm-opencl-runtime)
pkgver=3.0.0
pkgrel=1
pkgdesc='ROCm OpenCL™ Compatible Runtime'
arch=('x86_64')
url='https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime'
license=('MIT')
depends=("rocr-runtime>=${pkgver}" 'opencl-icd-loader')
makedepends=(mesa cmake ninja git)
provides=("$pkgname" 'opencl-driver')
conflicts=('rocm-opencl-git')
source=(
    "rocm-opencl-runtime::git+https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime#tag=roc-$pkgver"
    "llvm-roc-ocl-$pkgver.tar.gz::https://github.com/RadeonOpenCompute/llvm-project/archive/roc-ocl-$pkgver.tar.gz"
    "rocm-device-libs::git+https://github.com/RadeonOpenCompute/ROCm-Device-Libs#tag=roc-ocl-$pkgver"
    "rocm-comgr::git+https://github.com/RadeonOpenCompute/ROCm-CompilerSupport#tag=roc-$pkgver"
    "rocm-cmake::git+https://github.com/RadeonOpenCompute/rocm-cmake#tag=roc-$pkgver"
    "opencl-icd-loader::git+https://github.com/KhronosGroup/OpenCL-ICD-Loader#commit=${_opencl_icd_loader_commit}"
    'change-install-location.patch'
    'fix_rocm_opencl_build_order.patch'
)

sha256sums=('SKIP'
            '7715db8fd6322bdda4a75a9220cb6c747ca7e93e94ff158d7689d10f984efa45'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'f90a725a14dd972c86a6919e776e7a3a9dc1792031a9ba89d26a4068975034e6'
            '0f735299810e9e22cd57f5b96a7628a0f399d445a1f3bacefd8b890e84ca372f')

prepare() {
    cd "$srcdir/rocm-opencl-runtime"

    mkdir -p api/opencl/khronos
    ln -s "$srcdir/opencl-icd-loader" api/opencl/khronos/icd

    patch -Np1 -i "$srcdir/change-install-location.patch"
    # Fix build missing dependency
    # https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime/issues/64#issuecomment-473502685
    patch -Np1 -i "$srcdir/fix_rocm_opencl_build_order.patch"
}

build() {
    CMAKE_FLAGS='-DCMAKE_BUILD_TYPE=Release'

    # building LLVM/Clang requires ~1.5G per unit
    THREADS=$(( ($(getconf _PHYS_PAGES) * $(getconf PAGESIZE)) / 1610612736 ))
    if [ "$THREADS" -lt 1 ]; then
        THREADS=1
    fi
    msg2 "Building LLVM on $THREADS threads..."
    cd "$srcdir/llvm-project-roc-ocl-$pkgver"
    mkdir -p build && cd build
    cmake $CMAKE_FLAGS \
        -DLLVM_ENABLE_BINDINGS=OFF \
        -DLLVM_ENABLE_OCAMLDOC=OFF \
        -DLLVM_ENABLE_PROJECTS='clang;lld' \
        -DLLVM_TARGETS_TO_BUILD='AMDGPU' \
        -DOCAMLFIND=NO \
        -G Ninja \
        ../llvm
    ninja "-j$THREADS"
    LLVM_DIR="$srcdir/llvm-project-roc-ocl-$pkgver/build"
    CMAKE_FLAGS="$CMAKE_FLAGS -DLLVM_DIR='$LLVM_DIR'"

    if check_buildoption "ccache" "y"; then
        CMAKE_FLAGS="$CMAKE_FLAGS -DROCM_CCACHE_BUILD=ON"
    fi

    msg2 'Building prepare builtins...'
    cd "$srcdir/rocm-device-libs/utils"
    cmake $CMAKE_FLAGS -G Ninja ..
    ninja

    msg2 'Building device libs...'
    cd "$srcdir/rocm-device-libs"
    mkdir -p build && cd build
    cmake $CMAKE_FLAGS \
        -DCMAKE_INSTALL_PREFIX=/opt/rocm \
        -DPREPARE_BUILTINS="$srcdir/rocm-device-libs/utils/utils/prepare-builtins/prepare-builtins" \
        -G Ninja \
        ..
    ninja

    msg2 'Building rocm-comgr...'
    cd "$srcdir/rocm-comgr"
    mkdir -p build && cd build
    cmake $CMAKE_FLAGS \
        -DAMDDeviceLibs_DIR="$srcdir/rocm-device-libs/build" \
        -DClang_DIR="$LLVM_DIR/lib/cmake/clang" \
        -G Ninja \
        ../lib/comgr
    ninja

    msg2 'Building rocm-opencl-runtime...'
    cd "$srcdir/rocm-opencl-runtime"
    mkdir -p build && cd build
    cmake $CMAKE_FLAGS \
        -DCMAKE_INSTALL_PREFIX=/opt/rocm \
        -DCMAKE_MODULE_PATH="$srcdir/rocm-cmake/share/rocm/cmake" \
        -DUSE_COMGR_LIBRARY=ON \
        -Damd_comgr_DIR="$srcdir/rocm-comgr/build/lib/cmake/amd_comgr" \
        -G Ninja \
        ..
    ninja
}

package_rocm-device-libs() {
    DESTDIR="$pkgdir/" ninja -C "$srcdir/rocm-device-libs/build" install
}

package_rocm-opencl-runtime() {
    DESTDIR="$pkgdir/" ninja -C "$srcdir/rocm-opencl-runtime/build" install

    # OpenCL vendor file
    mkdir -p "$pkgdir/etc/OpenCL/vendors"
    echo "/opt/rocm/lib/x86_64/libamdocl64.so" > "$pkgdir/etc/OpenCL/vendors/rocm-opencl64.icd"

    mkdir -p "$pkgdir/etc/ld.so.conf.d"
    echo "/opt/rocm/lib" > "$pkgdir/etc/ld.so.conf.d/rocm-opencl.conf"
}
