# Maintainer: Jose Riha <jose1711 gmail com>
# Maintainer: Sebastian J. Bronner <waschtl@sbronner.com>
# Maintainer: Kevin Stolp <kevinstolp@gmail.com>
# Contributor: Patrick Jackson <PatrickSJackson gmail com>
# Contributor: Christoph Vigano <mail@cvigano.de>

pkgname=st
pkgver=0.9.2
pkgrel=1
pkgdesc='A simple virtual terminal emulator for X.'
arch=('i686' 'x86_64' 'armv7h' 'aarch64')
license=('MIT')
depends=(libxft)
url=https://st.suckless.org
source=(https://dl.suckless.org/$pkgname/$pkgname-$pkgver.tar.gz
        terminfo.patch
        README.terminfo.rst
        https://st.suckless.org/patches/scrollback/st-scrollback-0.8.5.diff
        https://st.suckless.org/patches/scrollback/st-scrollback-mouse-20220127-2c5edf2.diff
        https://st.suckless.org/patches/w3m/st-w3m-0.8.3.diff
        https://st.suckless.org/patches/fullscreen/st-fullscreen-0.8.5.diff
        st-fullscreen.patch
        https://st.suckless.org/patches/visualbell/st-visualbell-0.8.4.diff
        st-visualbell.patch
        https://st.suckless.org/patches/desktopentry/st-desktopentry-0.8.5.diff)
sha256sums=('6b215d4f472b21d6232f30f221117a777e24bcfee68955ddefb7426467f9494b'
            'f9deea445a5c6203a0e8e699f3c3b55e27275f17fb408562c4dd5d649edeea23'
            '95be3197f7de77a0fe2e4f527202e17e910ee24e1ed6bc39beb320a1304bb7e1'
            'dc7f5223b26fc813d91d4ae35bdaa54d63024cae9f18afd9b3594ba3399dfa55'
            '46ac9bcdbfeb0011533207cb0ab31657a3eb9196da1d0db346e6a9d1fc4b4f76'
            '8bd6fbd4c0a096c67a4a5f68585a66c93f0085b6ddea853a321b9be7316f91b3'
            'f889f4d92a627dc22a7f6f5b4c31e7aba0f2e5715cb9a10ef3bdd3b22d695ada'
            '7d5597b9bd5d193e9a19d41fb30d8c6602a6bc9804b8f5167533ac2b079c12c4'
            'd56c6438241759ecc30a987d853b524a2ba713db4ed4dfb2c000515f35047d90'
            '35b2b3756b4269a5282d516d57411128597848a684c150044551b51130bda57b'
            '9c59fdeaab64e3cc2db524ab6c12147476a9792c46a217e3a5f47029a42919da')
_sourcedir=$pkgname-$pkgver

prepare() {
  echo ">> apply terminfo"
  patch -d "$_sourcedir" -p 0 < terminfo.patch
  echo ">> apply scrollback.diff"
  patch -d "$_sourcedir" < st-scrollback-0.8.5.diff
  echo ">> apply mouse scrollback"
  patch -d "$_sourcedir" < st-scrollback-mouse-20220127-2c5edf2.diff
  echo ">> apply w3m"
  patch -d "$_sourcedir" < st-w3m-0.8.3.diff
  echo ">> apply fullscreen.patch"
  rm st-fullscreen-0.8.5.diff
  cp ../st-fullscreen-0.8.5.diff st-fullscreen-0.8.5.diff
  patch st-fullscreen-0.8.5.diff < st-fullscreen.patch
  echo ">> apply fullscreen"
  patch -d "$_sourcedir" < st-fullscreen-0.8.5.diff
  echo ">> apply visualbell"
  rm st-visualbell-0.8.4.diff
  cp ../st-visualbell-0.8.4.diff st-visualbell-0.8.4.diff
  patch st-visualbell-0.8.4.diff < st-visualbell.patch
  patch -d "$_sourcedir" < st-visualbell-0.8.4.diff
  echo ">> apply desktopentry"
  patch -d "$_sourcedir" < st-desktopentry-0.8.5.diff

  # This package provides a mechanism to provide a custom config.h. Multiple
  # configuration states are determined by the presence of two files in
  # $BUILDDIR:
  #
  # config.h  config.def.h  state
  # ========  ============  =====
  # absent    absent        Initial state. The user receives a message on how
  #                         to configure this package.
  # absent    present       The user was previously made aware of the
  #                         configuration options and has not made any
  #                         configuration changes. The package is built using
  #                         default values.
  # present                 The user has supplied his or her configuration. The
  #                         file will be copied to $_sourcedir and used during
  #                         build.
  #
  # After this test, config.def.h is copied from $_sourcedir to $BUILDDIR to
  # provide an up to date template for the user.
  if [ -e "$BUILDDIR/config.h" ]
  then
    cp "$BUILDDIR/config.h" "$_sourcedir"
  elif [ ! -e "$BUILDDIR/config.def.h" ]
  then
    echo \
      'This package can be configured in config.h. Copy the config.def.h that' \
      'was just placed into the package directory to config.h and modify it' \
      'to change the configuration. Or just leave it alone to continue to use' \
      'default values.'
  fi
  cp "$_sourcedir/config.def.h" "$BUILDDIR"
}

build() {
  make -C "$_sourcedir" X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  local shrdir="$pkgdir/usr/share"
  make -C "$_sourcedir" PREFIX=/usr DESTDIR="$pkgdir" install
  install -D -m 0644 -t "$shrdir/licenses/$pkgname" "$_sourcedir/LICENSE"
  install -D -m 0644 -t "$shrdir/doc/$pkgname" "$_sourcedir/README" README.terminfo.rst
  install -D -m 0644 -t "$shrdir/$pkgname" "$_sourcedir/st.info"
}
