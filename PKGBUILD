pkgname=hip
pkgver=1.4.17405
pkgrel=1
arch=(x86_64)

# Rather annoyingly, this depends on a _specific_ clang :/
source=(
    'http://releases.llvm.org/3.9.0/clang+llvm-3.9.0-x86_64-linux-gnu-ubuntu-16.04.tar.xz'
    "https://github.com/ROCm-Developer-Tools/HIP/archive/preview_${pkgver}.tar.gz"
)
md5sums=(
    '03687b22791b8c21813fc015dd507fc0'
    '25f17a99f0e4b82903c55c0a2f5406ac'
)

build() {
    cd $srcdir
    mkdir -p build
    cd build

    cmake \
      -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_CXX_COMPILER=clang++ \
      -DCMAKE_C_COMPILER=clang \
      -DHIPIFY_CLANG_LLVM_DIR=$srcdir/clang+llvm-3.9.0-x86_64-linux-gnu-ubuntu-16.04 \
      ../HIP-preview_${pkgver}

    make -j$(nproc)
}

package() {
    cd $srcdir/build
    make DESTDIR=${pkgdir} install

    # Nobody wants your source code, AMD..
    rm -R "${pkgdir}/opt/rocm/hip/src"

    # Put the finder script somewhere even vaguely convenient.
    mkdir -p "${pkgdir}/usr/share/cmake-3.9"
    cp -R "${pkgdir}/opt/rocm/hip/cmake" "${pkgdir}/usr/share/cmake-3.9"
    rm -R "${pkgdir}/opt/rocm/hip/cmake"

    # Synthesise an entry for /etc/profile.d to sort out the /bin stuff.
    mkdir -p "${pkgdir}/etc/profile.d"
    echo "export PATH=\$PATH:/opt/rocm/hip/bin" > "${pkgdir}/etc/profile.d/hip.sh"
    chmod a+x "${pkgdir}/etc/profile.d/hip.sh"
}
