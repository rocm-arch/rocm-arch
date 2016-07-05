# Maintainer:  Aqeel Akber <self@aqeelakber.com>
pkgname=hsa-runtime-git
pkgver=1.0.3.1.gfa0ef7e
pkgrel=1
pkgdesc="AMD Hetrogeneous System Architecture Runtime"
arch=('x86_64')
url="https://github.com/HSAFoundation/HSA-Runtime-AMD"
license=('custom:AMD')
provides=('hsa-runtime')
conflicts=('hsa-runtime')
source=(hsa-runtime::git+https://github.com/HSAFoundation/HSA-Runtime-AMD)
md5sums=('SKIP')


pkgver(){
  cd "$srcdir/hsa-runtime"
  git describe --long --tags | sed 's/release-v//;s/f//;s/-/./;s/-/./'
}

package() {
  #cd "$srcdir/$pkgname-$pkgver"
  cd "$srcdir/hsa-runtime"
  install -DTm 755 "LICENSE.txt" "$pkgdir/usr/share/licenses/hsa-runtime/LICENSE"
  install -DTm 755 "README.md" "$pkgdir/opt/hsa/README.md"

  cp -dr --no-preserve="ownership" "lib" "$pkgdir/opt/hsa/lib"
  cp -dr --no-preserve="ownership" "include" "$pkgdir/opt/hsa/include"
  cp -dr --no-preserve="ownership" "sample" "$pkgdir/opt/hsa/sample"
}

# vim:set ts=2 sw=2 et:
