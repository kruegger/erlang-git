# # Maintainer: Stephen Han <freddie_kruegger@hotmail.com>

pkgname=(erlang-git)
pkgver=18.3
pkgrel=1
_docver=18.3
arch=(x86_64 armv7h)
url='https://www.erlang.org'
license=(Apache)
makedepends=(git perl openssl-1.0)
source=("git+https://github.com/erlang/otp.git#tag=OTP-$pkgver"
        "$url/download/otp_doc_man_$_docver.tar.gz"
        epmd.conf epmd.service epmd.socket otp_src_18.patch)
sha256sums=('SKIP'
            '978be100e9016874921b3ad1a65ee46b7b6a1e597b8db2ec4b5ef436d4c9ecc2'
            '78ce5e67b21758c767d727e56b20502f75dc4385ff9b6c6db312d8e8506f2df2'
            'c220a6ec287579bd0705804232dc210889c54a3236b89733c9cbdab67b0698b5'
            '0f139f01547221ae091a868ce66d3bcc52c936facea3615c2932798d610bb74d'
            'db8313a3d039de4165eabe001fb56df35ec48880fb5c1b6bc4590f3b5dbf5b2f')

prepare() {
  cd otp
  patch --forward --strip=1 --input="$srcdir/otp_src_18.patch"
  ./otp_build autoconf
}

build() {
  cd otp
  ./configure \
    --prefix=/opt/erlang \
    --enable-builtin-zlib \
    --enable-smp-support \
    --with-ssl=/usr/lib/openssl-1.0     \
    --with-ssl-incl=/usr/include/openssl-1.0
  make
}

package_erlang-git() {
  pkgdesc='General-purpose concurrent functional programming language developed by Ericsson'
  depends=(ncurses openssl-1.0)
  optdepends=()
  provides=(erlang)
  conflicts=(erlang)

  make -C otp DESTDIR="$pkgdir" install

  # services and configuration
  install -Dm644 epmd.service "$pkgdir/usr/lib/systemd/system/epmd.service"
  install -Dm644 epmd.socket "$pkgdir/usr/lib/systemd/system/epmd.socket"
  install -Dm644 epmd.conf "$pkgdir/etc/conf.d/epmd"

  # readme and licenses
  install -Dm644 otp/README.md "$pkgdir/usr/share/doc/$pkgname/README.md"
  install -Dm644 COPYRIGHT "$pkgdir/usr/share/licenses/$pkgname/COPYRIGHT"
  install -Dm644 otp/LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  # man pages
  cp -r man "$pkgdir/usr/lib/erlang/"
}