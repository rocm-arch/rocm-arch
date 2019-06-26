# Maintainer: Ulysses R Ribeiro <ulyssesrr@gmail.com>
_opencl_icd_loader_commit="7433f2acbf5bbc400f26494ff1dc895da6265bef"

pkgname=rocm-opencl-runtime
pkgver=2.5.0
pkgrel=1
pkgdesc="ROCm OpenCL™ Compatible Runtime"
arch=('x86_64')
url="https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime"
license=('MIT')
groups=()
depends=("rocr-runtime>=${pkgver}" "opencl-icd-loader")
makedepends=(mesa cmake ninja gcc ocaml-findlib python2-z3 gtest git) 
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
    "7e7a7e761892dec27cf3f922cdd376a4b018f93832ecf8df75304695a03aa51a"
    "b590b217dbaf40bac89dff3e56f3dfb0c1a2205117e50e9cc007d01cf3b8e0f4"
    "fbb005d13c01020878dbc5546b57cce10bcdbd9b605d3c5a27955819e6c57507"
    "1611d533b670f2bd58ac1dce6cfb2a019ef6abdf119d657042e2a46c35dffe7e"
    "a910c565d249965be4eb0f5a00698b77181573271e3ec3f277c73297af9ed160"
    "d44b7161f9c3332c36c831119678d99fa9193056bcb79b2f85e1d8a22bece027"
    "b21a114e624231d1dfd95038def122b35d0ecbf9bc1c704b8c5aec88c5bf4842"
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
    
    # Discard AMD's OpenCL ICD Loader
    # https://wiki.archlinux.org/index.php/GPGPU#OpenCL_ICD_loader_(libOpenCL.so)
    rm -f $pkgdir/opt/rocm/opencl/lib/x86_64/libOpenCL.so*
}
