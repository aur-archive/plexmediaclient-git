# Maintainer: Daniel Wallace daniel.wallace@gatech.edu
pkgname=plexmediaclient-git
pkgver=20120310
pkgrel=1
pkgdesc="Plex Media Center on Linux"
arch=('x86_64' 'i686')
url="http://forums.plexapp.com/index.php/topic/34481-the-state-of-plex-media-center-on-linux/"
license=('GPL')
if test "$CARCH" == x86_64; then
  depends=("${depends[@]}" "lib32-boost-libs")
fi
depends=('alsa-lib' 'boost' 'boost-libs' 'cmake' 'curl' 'cvs' 'dbus-core' 'enca' 'faac' 'faad2' 'fontconfig' 'freetype2' 'fribidi' 'gd' 'glew' 'gperf' 'jasper' 'ldc' 'libass' 'libbluray' 'libcdio' 'libgl' 'libjpeg-turbo' 'libmad' 'libmicrohttpd' 'libmms' 'libmodplug' 'libmpeg2' 'libmysqlclient' 'libogg' 'libpng' 'libpulse' 'libsamplerate' 'libssh' 'libva' 'libvorbis' 'libxinerama' 'libxmu' 'libxrandr' 'libxrender' 'libxt' 'libxtst' 'lsb-release' 'lzo2' 'mesa' 'mesa-demos' 'moc' 'nasm' 'octave' 'openssl' 'pcre' 'pion-net' 'pmount' 'python-pysqlite' 'qmmp' 'rtmpdump' 'samba' 'sdl' 'sdl_gfx' 'sdl_image' 'sdl_mixer' 'smbclient' 'sqlite3' 'unzip' 'vlc' 'wavpack' 'xineramaproto' 'xorg-xdpyinfo') 
makedepends=('git' 'mingw32-binutils')
provides=(pmc)
conflicts=(xbmc)
replaces=(xmbc)
source=(libpng-patch.diff\
        lboost-patch.diff)
md5sums=('000ed1ec22219999a2c303f34ddfef35'
         '808fab4c681b27c68995333a6f1cd69f')

_gitroot=https://github.com/gewalker/plex-linux.git
_gitname=laika

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
  cd $srcdir
  patch -p0 < libpng-patch.diff
  cd "$srcdir/$_gitname-build"
  git submodule init
  git submodule update
  #
  # BUILD HERE
  #
  ./bootstrap
  ./configure --prefix=/usr
  cd $srcdir
  patch -p0 < lboost-patch.diff
  cd "$srcdir/$_gitname-build"
  make 
}

package() {
  cd "$srcdir/$_gitname-build"
  make DESTDIR="$pkgdir/" install
}

# vim:set ts=2 sw=2 et:
