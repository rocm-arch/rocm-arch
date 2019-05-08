# Submitter: Chris Kitching
# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=hip
pkgver=2.4.0
pkgrel=1
pkgdesc="Heterogeneous Interface for Portability ROCm"
url="https://github.com/ROCm-Developer-Tools/HIP"
arch=(x86_64)
makedepends=("hcc>=2.4.0" git cmake ninja python2)
source=("https://github.com/ROCm-Developer-Tools/HIP/archive/roc-$pkgver.tar.gz")
sha256sums=("019bef8b36493175545d3298f3ec3b9456be5128154a0f52201e6bcfbc971d86")

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  # A python2 script is invoked during cmake configuration that generates some headers
  sed -i "1s/python/python2/" "$srcdir/HIP-roc-$pkgver/hip_prof_gen.py"

  # TODO: fix libhip_hcc.so and libhip_hcc_static.a
  #       they contain references to $srcdir, I tried a bunch of things but nothing helps

  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$pkgdir/opt/rocm/hip" \
        -G Ninja \
        "$srcdir/HIP-roc-$pkgver"

  ninja
}

package() {
  ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/hip.conf
    /opt/rocm/hip/lib/
		EOF

  # Nobody wants your source code, AMD..
  rm -r "${pkgdir}/opt/rocm/hip/src"

  # Jakub: these two things below don't seem useful anymore, rest of the ecosystem
  #        looks in /opt/rocm for CMake finders, libraries etc.

  # Put the finder script somewhere even vaguely convenient.
  # mkdir -p "${pkgdir}/usr/share/cmake-3.9"
  # cp -R "${pkgdir}/opt/rocm/hip/cmake" "${pkgdir}/usr/share/cmake-3.9"
  # rm -R "${pkgdir}/opt/rocm/hip/cmake"

  # Synthesise an entry for /etc/profile.d to sort out the /bin stuff.
  # mkdir -p "${pkgdir}/etc/profile.d"
  # echo "export PATH=\$PATH:/opt/rocm/hip/bin" > "${pkgdir}/etc/profile.d/hip.sh"
  # chmod a+x "${pkgdir}/etc/profile.d/hip.sh"
}
