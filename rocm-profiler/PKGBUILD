# Maintainer: Ilya Elenskiy <elenskiy.ilya@gmail.com>
# Maintainer: Markus Näther <naetherm@informatik.uni-freiburg.de>
pkgname=rocm-profiler
_branch=3.0.0
pkgver=3.0.0
pkgrel=1
pkgdesc="ROC profiler library. Profiling with perf-counters and derived metrics for GFX8/GFX9."
arch=(x86_64)
url="https://github.com/ROCm-Developer-Tools/rocprofiler"
license=('MIT')
makedepends=(git cmake gcc ninja)
depends=('rocr-runtime>=3.0.0')
source=("https://github.com/ROCm-Developer-Tools/rocprofiler/archive/roc-$_branch.tar.gz")
sha256sums=('SKIP')

build() {
  mkdir -p "$srcdir/build"
  cd "$srcdir/build"
  cmake -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=$pkgdir/opt/rocm \
        -DCMAKE_PREFIX_PATH=/opt/rocm \
        -G Ninja \
        "$srcdir/rocprofiler-roc-$_branch"
  ninja
}

package() {
  ninja -C "$srcdir/build" install

  mkdir -p "$pkgdir/etc/ld.so.conf.d"
  cat <<-EOF > $pkgdir/etc/ld.so.conf.d/rocprofiler.conf
      /opt/rocm/rocprofiler/lib/
		EOF
}
