# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=rocfft
pkgver=0.9.4
pkgrel=1
pkgdesc="Next generation FFT implementation for ROCm"
url="https://github.com/ROCmSoftwarePlatform/rocfft"
arch=(x86_64)
depends=(boost fftw)
makedepends=("hcc>=2.6.0" git cmake ninja)
source=("https://github.com/ROCmSoftwarePlatform/rocfft/archive/v$pkgver.tar.gz")
sha256sums=("75fd804c051d274406f958142816f178c5a5d61f6bee1b03d5a827a6f4cb617d")

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"

  # build broken with stack protection
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
