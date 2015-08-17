# See http://wiki.archlinux.org/index.php/VCS_PKGBUILD_Guidelines
# for more information on packaging from GIT sources.

# Maintainer: Sven Schober <sschober@sssm.de>
pkgname=sundown-git
pkgver=20121218
pkgrel=1
pkgdesc="Standards compliant, fast, secure markdown processing library in C; includes sundown and smartypants executables."
arch=('any')
url="https://github.com/vmg/sundown"
license=('ISC')
makedepends=('git')
source=('LICENSE')
md5sums=('aa76e1df60e6a140b4061c6d33a6871a')

_gitroot=git://github.com/vmg/sundown.git
_gitname=sundown

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [[ -d "$_gitname" ]]; then
    cd "$_gitname" && git pull origin
    msg "The local files are updated."
  else
    git clone "$_gitroot" "$_gitname"
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  # remove unused `-Wl` option to `gcc`
  sed -i s/-Wl// Makefile

  make all
}

package() {
  install -D -m644 LICENSE "$pkgdir/usr/share/licenses/${pkgname}/LICENSE"

  cd "$srcdir/$_gitname-build"

  install -D -m755 smartypants "${pkgdir}/usr/bin/smartypants"
  install -D -m755 sundown "${pkgdir}/usr/bin/sundown"
  install -D -m755 libsundown.so.1 "${pkgdir}/usr/lib/libsundown.so.1"
  install -D -m755 src/markdown.h "${pkgdir}/usr/include/markdown.h"
  install -D -m755 src/buffer.h "${pkgdir}/usr/include/buffer.h"
  install -D -m755 src/autolink.h "${pkgdir}/usr/include/autolink.h"
  install -D -m755 html/html.h "${pkgdir}/usr/include/html.h"
  ln -rs "${pkgdir}/usr/lib/libsundown.so.1" "${pkgdir}/usr/lib/libsundown.so"
}

# vim:set ts=2 sw=2 et:
