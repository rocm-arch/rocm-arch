# Maintainer: Jakub Okoński <jakub@okonski.org>
pkgname=rocfft
pkgver=0.8.8
pkgrel=1
pkgdesc="Next generation FFT implementation for ROCm"
url="https://github.com/ROCmSoftwarePlatform/rocfft"
arch=(x86_64)
depends=(boost fftw)
makedepends=("hcc>=2.0.0" git cmake ninja)
source=("git+https://github.com/ROCmSoftwarePlatform/rocfft.git#tag=v0.8.8")
md5sums=("SKIP")

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
        "$srcdir/rocfft"

  ninja
}

package() {
  DESTDIR="$pkgdir" ninja -C "$srcdir/build" install

  mkdir -p $pkgdir/etc/ld.so.conf.d
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocfft.conf
    /opt/rocm/rocfft/lib
		EOF
}
