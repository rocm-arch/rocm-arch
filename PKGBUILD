# Maintainer: Ulysses R Ribeiro <ulyssesrr@gmail.com>
_opencl_icd_loader_commit="66ecca5dce2c4425a48bdb0cf0de606e4da43ab5"

pkgname=rocm-opencl-runtime
pkgver=2.4.0
pkgrel=1
pkgdesc="ROCm OpenCL™ Compatible Runtime"
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime"
license=('MIT')
groups=()
depends=("rocr-runtime>=${pkgver}")
makedepends=(cmake ninja gcc ocaml-findlib python2-z3 gtest) 
provides=("${pkgname}" "opencl-driver")
conflicts=("rocm-opencl-git")
replaces=()
backup=()
options=()
source=(
    "rocm-opencl-runtime-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime/archive/roc-${pkgver}.tar.gz"
    "rocm-opencl-driver-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/ROCm-OpenCL-Driver/archive/roc-${pkgver}.tar.gz"
    "llvm-roc-ocl-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/llvm/archive/roc-ocl-${pkgver}.tar.gz"
    "clang-roc-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/clang/archive/roc-${pkgver}.tar.gz"
    "lld-roc-ocl-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/lld/archive/roc-ocl-${pkgver}.tar.gz"
    "rocm-device-libs-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/ROCm-Device-Libs/archive/roc-${pkgver}.tar.gz"
    "OpenCL-ICD-Loader-${_opencl_icd_loader_commit}.zip::https://github.com/KhronosGroup/OpenCL-ICD-Loader/archive/${_opencl_icd_loader_commit}.zip"
    "fix_rocm_opencl_build_order.patch"
)

sha256sums=(
    "5dac0c7661de9914a70720b4082a293d6d22192ada836cc957310dbdeda32bc5"
    "f55fb74338a0dd19d9af40cb225b5f212326178d854c5b0ed85193f2edceb4bc"
    "4e8876444f15b18129dbcca286d3e8b1599f32d5ab4637fc481b6376a394817e"
    "a9a9f51394573a33bd6713bfeb51720928d51172888d08f6d52fa3fa64d200f4"
    "2c40b3aab20d7b31305354711700c595e42d45ff5afada5c7ceb5f8931fbbcb2"
    "5e7a45a2b72ccda01da27f3615e88f18ababa79f26b80602b894995c319ce42c"
    "788d27432ebcf9154b9585a1de52e707494486454991936ea837b6c1419fca43"
    "0f735299810e9e22cd57f5b96a7628a0f399d445a1f3bacefd8b890e84ca372f"
)

prepare() {
    # emulate repo opencl.xml layout
    cd $srcdir
    
    # <project path="opencl" name="ROCm-OpenCL-Runtime"/>
    ln -s $srcdir/ROCm-OpenCL-Runtime-roc-${pkgver} opencl
      
    #<project path="opencl/compiler/driver" name="ROCm-OpenCL-Driver"/>
    mkdir -p $srcdir/opencl/compiler/
    ln -s $srcdir/ROCm-OpenCL-Driver-roc-${pkgver} opencl/compiler/driver
    
    #<project path="opencl/compiler/llvm" name="llvm"/>
    mkdir -p $srcdir/opencl/compiler/
    ln -s $srcdir/llvm-roc-ocl-${pkgver} opencl/compiler/llvm
    
    #<project path="opencl/compiler/llvm/tools/clang" name="clang"/>
    mkdir -p $srcdir/opencl/compiler/llvm/tools/
    ln -s $srcdir/clang-roc-${pkgver} opencl/compiler/llvm/tools/clang
    
    #<project path="opencl/compiler/llvm/tools/lld" name="lld"/>
    mkdir -p $srcdir/opencl/compiler/llvm/tools/
    ln -s $srcdir/lld-roc-ocl-${pkgver} opencl/compiler/llvm/tools/lld
    
    #<project path="opencl/library/amdgcn" name="ROCm-Device-Libs"/>
    mkdir -p $srcdir/opencl/library/
    ln -s $srcdir/ROCm-Device-Libs-roc-${pkgver} opencl/library/amdgcn

    #<project path="opencl/api/opencl/khronos/icd" name="OpenCL-ICD-Loader"
    mkdir -p $srcdir/opencl/api/opencl/khronos/
    ln -s $srcdir/OpenCL-ICD-Loader-${_opencl_icd_loader_commit} opencl/api/opencl/khronos/icd
    
    # Fix build missing dependency
    # https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime/issues/64#issuecomment-473502685
    cd $srcdir/opencl
    patch --forward --strip=1 --input="${srcdir}/fix_rocm_opencl_build_order.patch"
}

build() {
    MEM_AVAIL=`cat /proc/meminfo | grep MemTotal | awk {'print $2'}`
    AVAIL_THREADS=`nproc`
    # require ~1536MB per thread
    MAX_THREADS=`echo $(( ${MEM_AVAIL} / $(( 1024 * 1536 ))))`
    if [ ${MAX_THREADS} -lt ${AVAIL_THREADS} ]; then
        NUM_BUILD_THREADS=${MAX_THREADS}
    else
        NUM_BUILD_THREADS=${AVAIL_THREADS}
    fi
    if [ ${NUM_BUILD_THREADS} -lt 1 ]; then
        NUM_BUILD_THREADS=1
    fi
    mkdir -p $srcdir/build
    cd $srcdir/build
    
    ## mimic AMD official binaries LDFLAGS
    #export LDFLAGS="-Wl,--no-as-needed"
    
    cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/opt/rocm/opencl \
        -DCLANG_ENABLE_STATIC_ANALYZER=ON \
        -G Ninja \
        $srcdir/opencl
    # -jNUM_BUILD_THREADS to avoid Out of Memory
    ninja -j$NUM_BUILD_THREADS
    
    # FIXME Check why lld has to built manually since 2.3.0
    ninja lld
}

package() {
    DESTDIR="$pkgdir/" ninja -C $srcdir/build install

    # OpenCL vendor file
    mkdir -p "$pkgdir/etc/OpenCL/vendors"
    echo "/opt/rocm/opencl/lib/x86_64/libamdocl64.so" > "$pkgdir/etc/OpenCL/vendors/rocm-opencl64.icd"

    mkdir -p "$pkgdir/etc/ld.so.conf.d/"
    echo "/opt/rocm/opencl/lib" > "$pkgdir/etc/ld.so.conf.d/rocm-opencl.conf"
    ln -s $srcdir/"/opt/rocm/opencl/lib/libOpenCL.so.1.2" "$pkgdir/opt/rocm/opencl/lib/libOpenCL.so" # not created automatically by ldconfig
}
