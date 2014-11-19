# $Id: PKGBUILD 113973 2014-07-01 10:51:04Z spupykin $
# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Dag Odenhall <dag.odenhall@gmail.com>
# Contributor: Grigorios Bouzakis <grbzks@gmail.com>

pkgname=dwm
pkgver=6.0
pkgrel=2
pkgdesc="A dynamic window manager for X"
url="http://dwm.suckless.org"
arch=('i686' 'x86_64')
license=('MIT')
options=(zipman)
depends=('libx11' 'libxinerama')
install=dwm.install
source=(http://dl.suckless.org/dwm/dwm-$pkgver.tar.gz
	config.h
	dwm.desktop
	bstack.c
	bstackhoriz.c)
_patches=(01-status-colors.diff
	02-centered-floating.diff
	03-xft-fonts.diff
	04-attachaside.diff
	05-emptyview.diff
	06-XkbKeycodeToKeysym_fix.diff
	07-pertag.diff)
source=(${source[@]} ${_patches[@]})
md5sums=('8bb00d4142259beb11e13473b81c0857'
         '4b2d1ec87914aca395f0359116e96c7e'
         '939f403a71b6e85261d09fc3412269ee'
         'e5704ef81be0800a9ee920946c612c1c'
         'f4281731e21a02fc13d827d876f0b705'
         '57b1a8f21b61c55f906d7cc075111613'
         '5679e23f529264484cf73e2e5f1fc5d3'
         'f28a81831c40d9d3bf34e931497b5721'
         '7c97902c84e193705d6ab0d8d5f0103b'
         'ddce74940a4de1b4e84c65fdbc678e49'
         '54cb8c816006b7a281335da0a3cc949f'
         'c3840bf8bb5904cb932dd6cdea5b4098')

build() {
  cd $srcdir/$pkgname-$pkgver

  for p in "${_patches[@]}"; do
	  echo " => $p"
	  patch < ../$p || return 1
  done

  cp $srcdir/config.h config.h
  cp $srcdir/bstack.c bstack.c
  cp $srcdir/bstackhoriz.c bstackhoriz.c

  sed -i 's/CPPFLAGS =/CPPFLAGS +=/g' config.mk
  sed -i 's/^CFLAGS = -g/#CFLAGS += -g/g' config.mk
  sed -i 's/^#CFLAGS = -std/CFLAGS += -std/g' config.mk
  sed -i 's/^LDFLAGS = -g/#LDFLAGS += -g/g' config.mk
  sed -i 's/^#LDFLAGS = -s/LDFLAGS += -s/g' config.mk
  sed -i 's$INCS =$& -I/usr/include/freetype2$g' config.mk
  sed -i 's$LIBS =$& -lXft$g' config.mk
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  cd $srcdir/$pkgname-$pkgver
  make PREFIX=/usr DESTDIR=$pkgdir install
  install -m644 -D LICENSE $pkgdir/usr/share/licenses/$pkgname/LICENSE
  install -m644 -D README $pkgdir/usr/share/doc/$pkgname/README
  install -m644 -D $srcdir/dwm.desktop $pkgdir/usr/share/xsessions/dwm.desktop
}
