# Maintainer: X0rg

pkgname=gnustep-gui-clang-git
_gitname=gnustep-gui
pkgver=7426.93e8ef8
pkgrel=3
pkgdesc="The GNUstep GUI class library, using Clang"
arch=('any')
url="http://www.gnustep.org/"
license=('GPL3' 'LGPL3')
groups=('gnustep-clang-git')
depends=('libtiff' 'libjpeg-turbo' 'libpng' 'giflib<5' 'libcups' 'icu')
makedepends=('git' 'clang' 'gnustep-opal-clang-git' 'audiofile' 'portaudio>=19')
optdepends=('aspell: GSspell.service support'
	'libsndfile'
	'libao')
conflicts=('gnustep-gui' 'gnustep-gui-svn')
options=('!emptydirs')
source=('git://github.com/gnustep/gnustep-gui.git')
md5sums=('SKIP')

pkgver() {
  cd $_gitname
  echo $(git rev-list --count HEAD).$(git rev-parse --short HEAD)
}

prepare() {
  cd $_gitname
  if [[ $(locale -a | grep french) == "french" ]];then msg2 "Patche le fichier 'NSBitmapImageRep+PNG.m'..."
  else
    msg2 "Patch 'NSBitmapImageRep+PNG.m' file..."
  fi
  (set -x ; sed -i 's|png_sizeof|sizeof|g' Source/NSBitmapImageRep+PNG.m)

  if [[ $(locale -a | grep french) == "french" ]];then msg2 "Désactive le service 'GSspell'..."
  else
    msg2 "Disable service 'GSspell'..."
  fi
  (set -x ; sed -i 's|SERVICE_NAME = GSspell|#SERVICE_NAME = GSspell|' ./Tools/GNUmakefile)
}

build() {
  cd $_gitname
  source /etc/profile.d/GNUstep.sh

  if [[ $(locale -a | grep french) == "french" ]];then msg2 "Exécute 'configure'..."
  else
    msg2 "Run 'configure'..."
  fi
  OBJCFLAGS=-fblocks CC=clang CXX=clang++ ./configure --disable-aspell --enable-libgif --prefix=/usr --sysconfdir=/etc/GNUstep

  if [[ $(locale -a | grep french) == "french" ]];then msg2 "Exécute 'make'..."
  else
    msg2 "Run 'make'..."
  fi
  make
}

# check() {
#   cd $_gitname
#   make check
# }

package() {
  cd $_gitname
  make DESTDIR="$pkgdir" install
}