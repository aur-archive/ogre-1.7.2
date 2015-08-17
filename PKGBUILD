# kkb110 : I copied from the official ogre package and modified a littlebit.

# $Id: PKGBUILD 47685 2011-05-24 19:41:50Z svenstaro $
# Maintainer: Sven-Hendrik Haase <sh@lutzhaase.com> 
pkgbase="ogre"
pkgname="ogre-1.7.2"
pkgname=('ogre-1.7.2')
pkgver=1.7.2
pkgrel=5
pkgdesc="A scene-oriented, flexible 3D engine written in C++ for python-ogre (since it only supports 1.7.2 yet)"
arch=('i686' 'x86_64')
url='http://www.ogre3d.org'
license=('custom:MIT')
depends=('boost-libs' 'freeimage' 'freetype2' 'libxaw' 'libxrandr' 
         'nvidia-cg-toolkit' 'mesa' 'zziplib' 'ois')
makedepends=('boost' 'cmake' 'doxygen' 'graphviz' 'ttf-dejavu' 'cppunit' 'intel-tbb' 'poco' 'boost')
optdepends=('cppunit: unit testing'
            'intel-tbb: better threading support'
            'poco: portability'
            'boost: for developing using ogre')
install=ogre.install
source=("http://downloads.sourceforge.net/${pkgbase}/${pkgbase}_src_v${pkgver//./-}.tar.bz2")
md5sums=('dd6574b8d906a74950c1e05633b2e96f')

conflicts=('ogre')
provides=('ogre')

build() {
  cd ${srcdir}/${pkgbase}_src_v${pkgver//./-}

  # get a clean build dir
  [[ -d build ]] && rm -rf build
	mkdir build
	cd build

  # generate CMake Makefile
  cmake .. \
 	  -DCMAKE_INSTALL_PREFIX=/usr \
	  -DOGRE_INSTALL_PLUGINS_HEADERS=TRUE \
	  -DOGRE_INSTALL_SAMPLES=TRUE \
	  -DOGRE_INSTALL_DOCS=TRUE \
    -DOGRE_INSTALL_MEDIA=TRUE \
	  -DOGRE_INSTALL_SAMPLES_SOURCE=TRUE \
    -DCMAKE_BUILD_TYPE=Release # set =Debug for debugging version

  # compile
  make

  # generate docs
  if [[ $(which dot) && $(which doxygen) ]]; then
    make doc
  fi
}

package_ogre-1.7.2() {
  optdepends=('ogre-docs: documentation')

  cd ${srcdir}/${pkgbase}_src_v${pkgver//./-}/build

  # install the bugger
  make DESTDIR=${pkgdir} install

  # fix up samples
  install -dm775 -o root -g users ${pkgdir}/opt/OGRE/samples/
  mv ${pkgdir}/usr/share/OGRE/*.cfg ${pkgdir}/opt/OGRE/samples/
  mv ${pkgdir}/usr/bin/SampleBrowser ${pkgdir}/opt/OGRE/samples/

  # make sample launcher
  echo "#!/bin/bash" > ${pkgdir}/usr/bin/OgreSampleBrowser
  echo "cd /opt/OGRE/samples && ./SampleBrowser" >> ${pkgdir}/usr/bin/OgreSampleBrowser
  chmod +x ${pkgdir}/usr/bin/OgreSampleBrowser
  
  # install license
  install -Dm644 ../Docs/License.html ${pkgdir}/usr/share/licenses/${pkgbase}/license.html

  # move docs out of this package
  mv ${pkgdir}/usr/share/OGRE/docs ${srcdir}/docs
}

#package_ogre-docs() {
#  pkgdesc="Documentation for ogre"
#  depends=()
#
#  cd ${srcdir}/${pkgbase}_src_v${pkgver//./-}/build
#
#  # move docs into this package
#  install -dm755 ${pkgdir}/usr/share/doc
#  mv ${srcdir}/docs ${pkgdir}/usr/share/doc/OGRE/
#
#  # symlink for docs
#  install -dm755 ${pkgdir}/usr/share/OGRE/
#  cd ${pkgdir}/usr/share
#  ln -s doc/OGRE/ OGRE/docs
#}

# vim:set ts=2 sw=2 et: