## Maintainer:     barfin <barfin@protonmail.com>
## Co-Maintainer:  Jaja <jaja@mailbox.org>

## pkginfo
pkgdesc="A fancy custom distribution of Valves Proton with various patches"
pkgname=proton-ge-custom-bin
pkgver=5.4_GE_3
pkgrel=1
arch=('x86_64')
license=('BSD' 'LGPL' 'zlib' 'MIT' 'MPL' 'custom')
provides=('proton')
depends=('python'
         'winetricks'
         'vulkan-icd-loader')
optdepends=('kdialog: KDE splash dialog support'
            'zenity: GNOME splash dialog support'
            'python-kivy: splash dialog support (big picture mode)'
            'steam: use proton with steam like intended'
            'lib32-vulkan-icd-loader: dxvk dependency for 32bit prefixes'
            'vulkan-driver: actually have a vulkan driver installed')

## makepkg options
options=('!strip')

## fix naming conventions, matching upstream
_pkgname=${pkgname//-bin/}
_pkgver=${pkgver//_/-}
_srcdir=Proton-${_pkgver}

## paths and files
_protondir=usr/share/steam/compatibilitytools.d/${_pkgver}
_licensedir=usr/share/licenses/${_pkgname}
_pfxdir=var/games/pfx_${_pkgver}
_execfile=usr/local/bin/proton
#_configfile=usr/local/etc/proton/user_settings.py
#backup=("${_configfile}")

## sources
url='https://github.com/GloriousEggroll/proton-ge-custom'
source=(${_pkgname}-${_pkgver}.tar.gz::"${url}/releases/download/${_pkgver}/${_srcdir}.tar.gz"
        "supplementary.tar.zst")
md5sums=('3cee50f910829928a4ce2567fe9a6d30'
         '1d52b4876be829e314fdf0c0ef8f7ed6')

prepare() {
## unpack wine
install -d ${_srcdir}/dist
bsdtar -xf ${_srcdir}/proton_dist.tar.gz -C ${_srcdir}/dist/
rm ${_srcdir}/proton_dist.tar.gz
}

build() {
## remove unused: dist_lock, extract_tarball(), make_default_prefix()
patch ${_srcdir}/proton patches/distlock-extract-defaultpfx.patch
## setup paths
#sed -i "s|self.path(\"user_settings.py\")|\"/${_configfile}\"|" ${_srcdir}/proton
sed -i "s|_proton=echo|_proton=/${_protondir}/proton|" ${srcdir}/launchers/proton.sh
sed -i "s|self.path(\"dist/share/default_pfx/\")|\"/${_pfxdir}/\"|" ${_srcdir}/proton
}

package() {
## create paths
install -d ${pkgdir}/${_protondir}/
install -d ${pkgdir}/${_licensedir}/
install -d --mode=2775 --group=games ${pkgdir}/${_pfxdir}/
chmod 0775 ${pkgdir}/${_pfxdir}/..
install -d ${pkgdir}/$(dirname ${_execfile})/
#install -d ${pkgdir}/$(dirname ${_configfile})/
## licenses
mv ${_srcdir}/LICENSE ${pkgdir}/${_licensedir}/license
mv ${_srcdir}/LICENSE.OFL ${pkgdir}/${_licensedir}/license_OFL
mv ${_srcdir}/protonfixes/LICENSE ${pkgdir}/${_licensedir}/license_protonfixes
## config files
#install --mode=0775 --group=games ${srcdir}/configs/user_settings.py ${pkgdir}/${_configfile}
## default pfx
mv ${_srcdir}/dist/share/default_pfx/* ${pkgdir}/${_pfxdir}
chown -R :games ${pkgdir}/${_pfxdir}
## executables
mv ${_srcdir}/* ${pkgdir}/${_protondir}
install --mode=0755 ${srcdir}/launchers/proton.sh ${pkgdir}/${_execfile}
}
