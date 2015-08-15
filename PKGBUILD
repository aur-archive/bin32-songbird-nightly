# Maintainer: ilikenwf/Matt Parnell <parwok@gmail.com>
# Contributor: Muhammad 'MJ' Jassim <UnbreakableMJ@gmail.com>
# Contributor: Aaron 'venisonslurpee' Laursen <venisonslurpee@gmail.com>
# Contributor: Techlive Zheng <techlivezheng at gmail dot com>

# I hacked this to just scrape the needed versioning info so that this doesn't
# have to be edited every day.
# - ilikenwf

pkgname=bin32-songbird-nightly
pkgver=2011.07.20
pkgrel=1
pkgdesc="Bleeding-edge nightly Songbird build. Automatically grabs latest build. Good until Nightingale is ready."
arch=('x86_64')
url="http://www.songbirdnest.com/"
license=('GPL2')
depends=('libxrender' 'libxext' 'libidl2' 'libx11' 'libxmu' 'libxt'
         'sqlite3' 'gtk2' 'nss'
         'lib32-gstreamer0.10'
         'lib32-gstreamer0.10-base'
         'lib32-gstreamer0.10-good')
optdepends=(
        'lib32-gstreamer0.10-bad: support for more formats'
        'lib32-gstreamer0.10-ugly: support for more formats')
provides=('songbird')
conflicts=('songbird' 'songbird-svn' 'songbird-bin64')
install=bin32-songbird-nightly.install
source=(Songbird.desktop songbird-launcher.sh)
md5sums=('02ffd66cd34054654135b25d8f82b906'
         'afc2ad4ad4954e98af3703a1a07cd574')

build() {
  # Why update daily when we can just scrape the url for the latest version?  -ilikenwf
  VERSION=`wget -qO- "http://developer.songbirdnest.com/builds/trunk/latest/" \
      | grep -o 'href=\"Songbird_.*_linux-i686.tar.gz\"' | sed 's/href=\"//' | sed 's/gz\".*/gz/'`

  cd ${srcdir}

  if [ -a `echo ${VERSION} | grep -o Songbird.*.tar.gz` ];then
    msg "File exists, no need to download."
  else
    wget http://developer.songbirdnest.com/builds/trunk/latest/${VERSION}
    wget http://developer.songbirdnest.com/builds/trunk/latest/${VERSION}.md5
  fi

  msg "Checking md5sum of Songbird archive..."
  md5sum -c ${VERSION}.md5

  msg "Building."

  tar xf ${VERSION}

  # Use the proper number for the pkgver and pkgrel
  # Tricks to made AUR not pick up these shell command as pkgver and pkgrel
  : && pkgver=$(echo ${VERSION} | sed 's/Songbird\_//' | sed 's/\-.*_linux.*\.tar\.gz//')
  : && pkgrel=$(echo ${VERSION} | grep -o '[0-9][0-9][0-9][0-9]')

  cd Songbird_build-${pkgrel}

  install -d --group=users ${pkgdir}/opt/songbird
  cp -a ${srcdir}/Songbird_build-${pkgrel}/* ${pkgdir}/opt/songbird
  chmod 755 ${pkgdir}/opt/songbird/songbird
  chmod 755 ${pkgdir}/opt/songbird/songbird-bin
  chmod 755 ${pkgdir}/opt/songbird/xulrunner/xulrunner
  chmod 755 ${pkgdir}/opt/songbird/xulrunner/xulrunner-bin
  chmod -R a+r ${pkgdir}/opt/songbird
  install -D ${srcdir}/Songbird_build-${pkgrel}/chrome/icons/default/default.xpm \
         ${pkgdir}/usr/share/pixmaps/Songbird.xpm
  install -D -m644 ${srcdir}/Songbird.desktop \
         ${pkgdir}/usr/share/applications/Songbird.desktop
  install --directory ${pkgdir}/usr/bin

  ## fixing the problem with system-wide installs:
  find ${pkgdir}/opt/songbird -type d -exec chmod 755 {} \;
  install --directory ${pkgdir}/usr/bin
  install ${srcdir}/songbird-launcher.sh ${pkgdir}/usr/bin/songbird
}
