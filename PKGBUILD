# Maintainer : speps <speps at aur dot archlinux dot org>
# Contributor: Bernardo Barros <bernardobarros@gmail.com>

pkgname=muse2
pkgver=2.1.2
pkgrel=1
pkgdesc="A MIDI/Audio sequencer with recording and editing capabilities"
arch=('i686' 'x86_64')
url="http://muse-sequencer.org/"
license=('GPL')
depends=('qt4' 'fluidsynth' 'lash' 'liblo')
makedepends=('cmake' 'doxygen' 'graphviz' 'ladspa' 'dssi')
optdepends=('dssi: DSSI plugins'
            'ladspa: LADPLA plugins')
install="$pkgname.install"
source=("http://downloads.sourceforge.net/lmuse/muse-$pkgver.tar.gz")
md5sums=('ad917335ac05a3d62e3cd073af901001')

build() {
  cd "$srcdir/muse-$pkgver"

  # build dir
  [ -d bld ] || mkdir bld && cd bld

  cmake -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_BUILD_TYPE=release \
        -DPYTHON_INCLUDE_DIR=/usr/include/python2.7 \
        -DPYTHON_LIBRARY=/usr/lib/libpython2.7.so \
        -DQT_QMAKE_EXECUTABLE=/usr/bin/qmake-qt4 \
        -DCMAKE_ENABLE_VST_VESTIGE=1 \
        -DCMAKE_ENABLE_VST_NATIVE=1 \
        -DENABLE_PYTHON=1 \
        -DENABLE_FLUID=1 \
        -DENABLE_DSSI=1 \
        -DENABLE_LASH=1 \
        -DENABLE_OSC=1 ..
  make
}

package() {
  cd "$srcdir/muse-$pkgver/bld"
  make DESTDIR="$pkgdir" install

  # prevent conflicts with Muse qt3 ..
  cd "$pkgdir/usr/share/applications"
  mv muse.desktop $pkgname.desktop
  sed -i "s|sE|& 2|" $pkgname.desktop

  # .. and oomidi grepmidi bin
  cd "$pkgdir/usr/bin"
  mv grepmidi $pkgname-grepmidi

  # grepmidi man
  cd "$pkgdir/usr/share/man/man1"
  mv grepmidi.1 $pkgname-grepmidi.1

  # python2 Fixes
  sed -i "s/\#\!.*python/&2/" \
    `grep -rl "\#\!.*python" "$pkgdir"`
}
