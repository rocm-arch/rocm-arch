pkgname=openmp-extras
pkgdesc='Radeon Open Compute - openmp-extras with flang (OpenMP AMD GPU Offloading)'
pkgver=4.5.0
pkgrel=1
arch=('x86_64')
url='https://github.com/ROCm-Developer-Tools/aomp'
license=('custom:Apache 2.0 with LLVM Exception')
depends=(hsa-rocr rocm-llvm)
makedepends=(cmake python python-pip python-wheel ninja gawk)
llvm_url='https://github.com/RadeonOpenCompute/llvm-project'
dlibs_url='https://github.com/RadeonOpenCompute/ROCm-Device-Libs'
flang_url='https://github.com/ROCm-Developer-Tools/flang'
extras_url='https://github.com/ROCm-Developer-Tools/aomp-extras'
source=("llvm-amdgpu-${pkgver}.tar.gz::$llvm_url/archive/rocm-$pkgver.tar.gz"
        "aomp-$pkgver.tar.gz::$url/archive/rocm-$pkgver.tar.gz"
        "aomp-extras-$pkgver.tar.gz::$extras_url/archive/rocm-$pkgver.tar.gz"
        "aomp-device-libs-$pkgver.tar.gz::$dlibs_url/archive/rocm-$pkgver.tar.gz"
        "flang-$pkgver.tar.gz::$flang_url/archive/rocm-$pkgver.tar.gz"
        "aomp-gcc8-only-for-cuda.patch"
        "aomp-openmp-buildpath.patch"
        "aomp-openmp-llvm.patch"
        "aomp-flang-decouple-out-dir-and-rocm-install.patch"
        "aomp-flang-libomp-path.patch"
        "aomp-flang-remove-sha.patch"
        "aomp-extras-compiler-in-rocm-dir.patch"
        )
sha256sums=('e33d08c8ae42a3ba394dceb8938263cd14ba45e9603b18f3502c3344814ca296'
            '27a5794b5885c61dc6f63cec36673b37deb029754d3b2fd3e1b21239efffa96a'
            '66d4d0cb4a1a05edb5f13599d8c06de4e10daf561201273cc56dc907139cb894'
            '78412fb10ceb215952b5cc722ed08fa82501b5848d599dc00744ae1bdc196f77'
            '3d7277fd658e51e7e43272c4b319e733c18f5a6d11f739aaec0df758a720809e'
            '705a7103c3aeff514e5645c130786172961c54673dfdd772caece3b5e7536088'
            'f7ed1704ffb095bbe8512b1c567a111936685d35f64123c786194e4239277251'
            'f673b6d5d2b7bd1a37cfc6e1015c75bc484504301de35a599c8a4034596d0740'
            'e82a4f065cc259095bf96778b913b97fe39d7c207e4e25ccf59d8fa264014262'
            'ff3c3e56bfc11c0c9a6ab5c5392168da06aed3b4a21cdfcf7a149d12a69e2906'
            '565af54e3079c130c87ebe036d91db21631e01ff0435f80bc54dbb55e8b02229'
            'f8f57cdcc4ddf535323f4f84a4a4a4fa830fbad72b19c7ea45d74fa5579ee72f'
            )
options=(staticlibs)

prepare() {

    ln -f -s $srcdir/llvm-project-rocm-$pkgver $srcdir/llvm-project
    ln -f -s $srcdir/flang-rocm-$pkgver $srcdir/flang
    ln -f -s $srcdir/aomp-extras-rocm-$pkgver $srcdir/aomp-extras

    cd $srcdir/aomp-rocm-$pkgver
    patch -Np1 < $srcdir/aomp-gcc8-only-for-cuda.patch
    patch -Np1 < $srcdir/aomp-openmp-buildpath.patch
    patch -Np1 < $srcdir/aomp-openmp-llvm.patch
    patch -Np1 < $srcdir/aomp-flang-decouple-out-dir-and-rocm-install.patch
    patch -Np1 < $srcdir/aomp-flang-libomp-path.patch

    cd $srcdir/flang
    patch -Np1 < $srcdir/aomp-flang-remove-sha.patch

    cd $srcdir/aomp-extras
    patch -Np1 < $srcdir/aomp-extras-compiler-in-rocm-dir.patch

}

build() {

    export OMPEXTRA_DIR=$srcdir/out/openmp-extras
    export OUT_DIR=$OMPEXTRA_DIR
    export AOMP=$OUT_DIR/llvm
    export AOMP_STANDALONE_BUILD=0
    export AOMP_REPOS=$srcdir
    export ROCM_DIR=/opt/rocm
    export DEVICELIBS_ROOT=$srcdir/ROCm-Device-Libs-rocm-$pkgver
    export LLVM_PROJECT_ROOT=$srcdir/llvm-project

    cd $srcdir/aomp-rocm-$pkgver/bin
    ./build_extras.sh
    ./build_extras.sh install

    ./build_openmp.sh
    ./build_openmp.sh install

    ./build_pgmath.sh
    ./build_pgmath.sh install

    ./build_flang.sh
    ./build_flang.sh install

    ./build_flang_runtime.sh
    ./build_flang_runtime.sh install
}


package() {

    mkdir -p $pkgdir/opt/rocm/
    cp -r $srcdir/out/openmp-extras/* $pkgdir/opt/rocm/
}
