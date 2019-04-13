# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=rocfft
pkgver=0.9.1
pkgrel=1
pkgdesc="Next generation FFT implementation for ROCm"
url="https://github.com/ROCmSoftwarePlatform/rocfft"
arch=(x86_64)
depends=(boost fftw)
makedepends=("hcc>=2.3.0" git cmake ninja)
source=("https://github.com/ROCmSoftwarePlatform/rocfft/archive/v$pkgver.tar.gz")
sha256sums=("fa4cb36409e34f85aa2de8d44b6990afe2c8b9fef7793c50f320f2e114fb0c09")

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  # build broken build with stack protection
  export CXXFLAGS=$(echo $CXXFLAGS | sed -e 's/-fstack-protector-strong//')
  export CFLAGS=$(echo $CFLAGS | sed -e 's/-fstack-protector-strong//')
  export CPPFLAGS=$(echo $CPPFLAGS | sed -e 's/-fstack-protector-strong//')

  # compile with HCC
  export CXX=/opt/rocm/hcc/bin/hcc

  cmake -DCMAKE_BUILD_TYPE=Release \
        -G Ninja \
        "$srcdir/rocFFT-$pkgver"

  ninja
}

package() {
  DESTDIR="$pkgdir" ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocfft.conf
    /opt/rocm/rocfft/lib
		EOF
}
