# Maintainer: Daniel Seymour <dannyseeless at gmail dot com>

pkgname=mediabrowser-server-git
pkgver=r5648.21a2e06
pkgrel=1
pkgdesc="MediaBrowser Server is a home media server built using other popular open source technologies."
arch=('i686' 'x86_64' 'armv6h')
url="http://www.mediabrowser.tv"
license=('GPL')
groups=()
depends=('mono' 'libmediainfo' 'sqlite' 'ffmpeg' 'imagemagick')
makedepends=('git' 'imagemagick')
optdepends=()
conflicts=('mediabrowser-server' 'mediabrowser-server-beta')
provides=('mediabrowser-server')
install=mediabrowser-server.install
source=("git+https://github.com/MediaBrowser/MediaBrowser#branch=dev" 
        "mediabrowser-server.service" "mediabrowser-server" 
        "mediabrowser-server.conf")
md5sums=('SKIP' 'SKIP' 'SKIP' 'SKIP')

pkgver() {
  cd "MediaBrowser"
  (  
    set -o pipefail
    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
  )
}

prepare() {
  MAGICKWAND=$(ldconfig -p | grep MagickWand.*.so$ | cut -d" " -f4)
  sed -i "s/libMagickWand-6.Q8.so/${MAGICKWAND##*/}/" ${srcdir}/MediaBrowser/MediaBrowser.Server.Mono/ImageMagickSharp.dll.config
}

build(){
  cd ${srcdir}/MediaBrowser
  xbuild /p:Configuration="Release Mono" /p:Platform="Any CPU" /p:OutputPath="${srcdir}/usr/lib/mediabrowser-server" /t:build MediaBrowser.Mono.sln
  rm -rf ${srcdir}/MediaBrowser
}

package() {
  install -Dm644 ${srcdir}/mediabrowser-server.conf ${pkgdir}/etc/conf.d/mediabrowser-server.conf
  install -Dm755 ${srcdir}/mediabrowser-server ${pkgdir}/usr/bin/mediabrowser-server
  install -Dm644 ${srcdir}/mediabrowser-server.service ${pkgdir}/usr/lib/systemd/system/mediabrowser-server.service
  cp -r ${srcdir}/usr/lib/mediabrowser-server ${pkgdir}/usr/lib
}
