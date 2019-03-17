# Maintainer: Ulysses R Ribeiro <ulyssesrr@gmail.com>
_opencl_icd_loader_commit="44f384ae624675d6b0b92ce39a97cb7f4899d92a"

pkgname=rocm-opencl-runtime
pkgver=2.2.0
pkgrel=1
pkgdesc="ROCm OpenCL™ Compatible Runtime"
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime"
license=('MIT')
groups=()
depends=("rocr-runtime>=${pkgver}")
makedepends=(cmake ninja gcc ocaml-findlib python2-z3 gtest) 
provides=("${pkgname}")
conflicts=("${pkgname}" "rocm-opencl-git")
replaces=()
backup=()
options=()
source=(
    "rocm-opencl-runtime-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime/archive/roc-${pkgver}.tar.gz"
    "rocm-opencl-driver-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/ROCm-OpenCL-Driver/archive/roc-${pkgver}.tar.gz"
    "llvm-roc-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/llvm/archive/roc-${pkgver}.tar.gz"
    "clang-roc-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/clang/archive/roc-${pkgver}.tar.gz"
    "lld-roc-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/lld/archive/roc-${pkgver}.tar.gz"
    "rocm-device-libs-${pkgver}.tar.gz::https://github.com/RadeonOpenCompute/ROCm-Device-Libs/archive/roc-${pkgver}.tar.gz"
    "OpenCL-ICD-Loader-${_opencl_icd_loader_commit}.zip::https://github.com/KhronosGroup/OpenCL-ICD-Loader/archive/${_opencl_icd_loader_commit}.zip"
)

sha256sums=(
    "3bb99867ae962f5b47da03e23f39abc6e0ff5e3b9ef49646b139d14e1def11ae"
    "cda84f7fcff46ac1e36399fd919512573b89632c692ae93cd9d846f8067aa48a"
    "d46116a8adbacab6aa669704994d121d94264ed69851238bf51a26432e5dbc9a"
    "4900cabd4735419785e4919e6ec1c7fe1792a8934b0413be87d469809255d287"
    "6f39e600c6213c278f8cbf4df61b3b132915a9c68841b41ed8decd4013e03cc4"
    "65c165d33898a7782b60cfe45d560d657b82e350d35035d11b759ee6d7dec190"
    "a8a5b2305f589f315403ca8390ab2ec57acbc99aae4fcabe7bb9632a6b11e89f"
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
    ln -s $srcdir/llvm-roc-${pkgver} opencl/compiler/llvm
    
    #<project path="opencl/compiler/llvm/tools/clang" name="clang"/>
    mkdir -p $srcdir/opencl/compiler/llvm/tools/
    ln -s $srcdir/clang-roc-${pkgver} opencl/compiler/llvm/tools/clang
    
    #<project path="opencl/compiler/llvm/tools/lld" name="lld"/>
    mkdir -p $srcdir/opencl/compiler/llvm/tools/
    ln -s $srcdir/lld-roc-${pkgver} opencl/compiler/llvm/tools/lld
    
    #<project path="opencl/library/amdgcn" name="ROCm-Device-Libs"/>
    mkdir -p $srcdir/opencl/library/
    ln -s $srcdir/ROCm-Device-Libs-roc-${pkgver} opencl/library/amdgcn

    #<project path="opencl/api/opencl/khronos/icd" name="OpenCL-ICD-Loader"
    mkdir -p $srcdir/opencl/api/opencl/khronos/
    ln -s $srcdir/OpenCL-ICD-Loader-${_opencl_icd_loader_commit} opencl/api/opencl/khronos/icd
    
    # Fix build missing dependency
    # https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime/issues/64#issuecomment-473502685
    cd $srcdir/opencl
    patch --forward --strip=1 --input="${srcdir}/../fix_rocm_opencl_build_order.patch"
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
    cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/opt/rocm/opencl \
        -DCLANG_ENABLE_STATIC_ANALYZER=ON \
        -G Ninja \
        $srcdir/opencl
    # -jNUM_BUILD_THREADS to avoid Out of Memory
    ninja -j$NUM_BUILD_THREADS
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
