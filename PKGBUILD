# Maintainer: Andy Weidenbaum <archbaum@gmail.com>
# Contributor: Daniel YC Lin <dlin.tw at gmail.com>
# Contributor: boypt <pentie at gmail.com>
# Contributor: Kyle Keen <keenerd@gmail.com>

pkgname=zeromq-git
pkgver=4.0.4
pkgrel=1
pkgdesc="ZeroMQ core engine in C++"
arch=('i686' 'x86_64')
depends=('gcc-libs' 'libpgm')
makedepends=('autoconf' 'automake' 'gcc' 'git' 'libtool' 'make' 'pkg-config')
url="https://github.com/zeromq/libzmq"
license=('LGPL3')
options=('staticlibs')
source=(${pkgname%-git}::http://download.zeromq.org/zeromq-4.0.4.tar.gz)
sha256sums=('SKIP')
provides=('zeromq')
conflicts=('zeromq')

pkgver() {
  echo 4.0.4
}

build() {
  cd ${pkgname%-git}-4.0.4

  msg2 'Building...'
  ./autogen.sh
  ./configure \
    --prefix=/usr \
    --sbindir=/usr/bin \
    --libexecdir=/usr/lib/zeromq \
    --sysconfdir=/etc \
    --sharedstatedir=/usr/share/zeromq \
    --localstatedir=/var/lib/zeromq \
    --disable-perf \
    --enable-static \
    --with-gnu-ld \
    --without-pgm \
    --without-docs
  make
}

package() {
  cd ${pkgname%-git}-4.0.4

  msg2 'Installing license...'
  install -Dm 644 COPYING* -t "$pkgdir/usr/share/licenses/zeromq"

  msg2 'Installing...'
  make DESTDIR="$pkgdir" install


  msg2 'Renaming binaries...'
  for _bin in $(find "$pkgdir/usr/bin" -type f -printf '%f\n'); do
    mv "$pkgdir/usr/bin/$_bin" "$pkgdir/usr/bin/zmq_$_bin"
  done

  msg2 'Cleaning up pkgdir...'
  find "$pkgdir" -type d -name .git -exec rm -r '{}' +
  find "$pkgdir" -type f -name .gitignore -exec rm -r '{}' +
}
