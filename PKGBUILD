# Maintainer: Limao <luolimao+AUR@gmail.com>
# Contributor: Jonathan Wiersma <arch aur@jonw.org>

pkgname=helixplayer-nightly-bin
_pkgname=hxplay
pkgver=11.1.1.2953
_pkgdate=20100721
_pkgdlhash=302d6a4e118b0d10b015
pkgrel=1
pkgdesc="Open source media player based on the Helix DNA Client media engine"
arch=(i686 x86_64)
_arch=linux-2.2-libc6-gcc32-i586
url=http://forms.helixcommunity.org/helix/builds/
license=(GPL custom:RPSL custom:RCSL)
depends=(desktop-file-utils hicolor-icon-theme lib32-{alsa-lib,gtk2,libxv,libstdc++5})
[[ $CARCH == "i686" ]] && depends=(${depends[@]/lib32-/})
provides=(helixplayer=$pkgver)
conflicts=(helixplayer {lib32-,}realplayer{,-nightly-bin})
install=$_pkgname.install
source=(http://software-dl.real.com/$_pkgdlhash/helix/$_pkgdate/player_all-${_pkgname}_gtk_current-$_pkgdate-$_arch/$_pkgname-$pkgver-$_arch.tar.bz2 $_pkgname.desktop)
sha256sums=('ebbd25a8ece6440bb7737dcbde237fe5e92aeb6608572bfa1c5dfa403dc3a9af'
    '069beab6f9b10acd985741ac723163bdcaf009056d7bf89145c2718bb1a8a18c')
sha512sums=('977612958d79d282dbeab4a88b6ec8c894c95bf45a62d3202a7e9b83c8fd2046037c542fba2aac5ea2547ae0273d1f0012da63f77db2ce5a88532d817a0ecff8'
    'dfed4e3ae5de290574fa0f1acc1186dcf42d6a939945017ca865b3ee55b2b53a5f5d798575029f929b6d0267beb3c05f1503f3ccbb79fa9b1ae95a60ad679db2')

package() {
    cd "$srcdir"

    # Install shared libs
    for _file in {codecs,common,plugins}/*.so; do
        install -Dm755 $_file "$pkgdir"/opt/$pkgname/$_file
    done

    # Install executables
    install -Dm755 $_pkgname $_pkgname.bin "$pkgdir"/opt/$pkgname/
    install -Dm755 Bin/setup $"pkgdir"/opt/$pkgname/bin/setup
    install -d "$pkgdir"/usr/bin/
    ln -s /opt/$pkgname/$_pkgname "$pkgdir"/usr/bin/$_pkgname

    # Install mozilla plugins
    install -d "$pkgdir"/usr/lib/mozilla/plugins/
    install -Dm755 mozilla/*.{so,xpt} "$pkgdir"/usr/lib/mozilla/plugins/

    # Install License and extras
    install -Dm644 LICENSE "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
    install -m644 LICENSE README "$pkgdir"/opt/$pkgname/

    ## Time to SHARE
    cd share/
    # Install selected files from share directory
    find -type f -execdir install -Dm644 '{}' "$pkgdir"/opt/$pkgname/'{}' \;

    for _file in locale/*/{LICENSE,README}; do
        install -Dm644 $_file "$pkgdir"/opt/$pkgname/$_file
    done

    # Install Icons
    for _res in 16 192 32 48; do
        install -Dm644 icons/${_pkgname}_${_res}x$_res.png "$pkgdir"/usr/share/icons/hicolor/${_res}x$_res/apps/$_pkgname.png
    done

    # Install desktop extras
    install -Dm644 $_pkgname.png "$pkgdir"/usr/share/pixmaps/$_pkgname.png
    install -Dm644 $_pkgname.applications "$pkgdir"/usr/share/application-registry/$_pkgname.applications
    desktop-file-install ../$_pkgname.desktop --dir "$pkgdir"/usr/share/applications/

    install -d "$pkgdir"/usr/share/mime-info/
    install -m644 $_pkgname.{keys,mime} "$pkgdir"/usr/share/mime-info/

    # Install Locales
    cd locale/
    for _locale in *; do
        _loc="$pkgdir"/usr/share/locale/$_locale/LC_MESSAGES/
        install -d "$_loc"
        install -m644 $_locale/*.mo "$_loc"
    done
}
