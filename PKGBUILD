# Maintainer: Det <nimetonmaili g-mail>
# Contributors: t3ddy, Lex Rivera aka x-demon, ruario

# Check for new Linux releases in: http://googlechromereleases.blogspot.com/search/label/Beta%20updates
# or use: $ curl -s https://dl.google.com/linux/chrome/rpm/stable/x86_64/repodata/other.xml.gz | gzip -df | awk -F\" '/pkgid/{ sub(".*-","",$4); print $4": "$10 }'

pkgname=google-chrome-beta
pkgver=49.0.2623.47
pkgrel=1
pkgdesc="An attempt at creating a safer, faster, and more stable browser (Beta Channel)"
arch=('x86_64')
url="https://www.google.com/chrome/index.html"
license=('custom:chrome')
depends=('alsa-lib' 'desktop-file-utils' 'flac' 'gconf' 'gtk2' 'harfbuzz' 'harfbuzz-icu' 'hicolor-icon-theme'
         'icu' 'libpng' 'libxss' 'libxtst' 'nss' 'opus' 'snappy' 'speech-dispatcher' 'ttf-font' 'xdg-utils')
optdepends=('kdebase-kdialog: needed for file dialogs in KDE'
            'ttf-liberation: fix fonts for some PDFs')
makedepends=('pacman>=4.2.0')
provides=('google-chrome' 'pepper-flash')
options=('!emptydirs' '!strip')
install=$pkgname.install
_channel=beta
source=("google-chrome-${_channel}_${pkgver}_amd64.deb::https://dl.google.com/linux/direct/google-chrome-${_channel}_current_amd64.deb"
        'google-chrome-beta'
        'eula_text.html')
md5sums=('9a5eae2ee3673b3fda95f3831efb5628'
         'ca16d5162eed85b1ba4e6b9fc37f9e35'
         'b7e752f549b215ac77f284b6486794b6')

package() {
  msg2 "Extracting the data.tar.xz..."
  bsdtar -xf data.tar.xz -C "$pkgdir/"

  msg2 "Moving stuff in place..."
  # Launcher
  install -m755 google-chrome-$_channel "$pkgdir"/usr/bin/

  # Icons
  for i in 16x16 22x22 24x24 32x32 48x48 64x64 128x128 256x256; do
    install -Dm644 "$pkgdir"/opt/google/chrome-$_channel/product_logo_${i/x*}.png \
                   "$pkgdir"/usr/share/icons/hicolor/$i/apps/google-chrome-$_channel.png
  done

  # Man page
  gzip "$pkgdir"/usr/share/man/man1/google-chrome-$_channel.1

  # License
  install -Dm644 eula_text.html "$pkgdir"/usr/share/licenses/google-chrome-$_channel/eula_text.html

  msg2 "Fixing Chrome icon resolution..."
  sed -i "/Exec=/i\StartupWMClass=google-chrome-$_channel" "$pkgdir"/usr/share/applications/google-chrome-$_channel.desktop

  msg2 "Removing unnecessities (e.g. Debian Cron job)..."
  rm -r "$pkgdir"/etc/cron.daily/ "$pkgdir"/opt/google/chrome-$_channel/cron/
  rm "$pkgdir"/opt/google/chrome-$_channel/product_logo_*.png
}
