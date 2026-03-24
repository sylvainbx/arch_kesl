# Maintainer: sylvainb
pkgname=('kesl')
pkgver=12.4.0.1225
_pkgverbuild=$(echo ${pkgver} | cut -d "." -f 4)
_pkgver=$(echo ${pkgver} | cut -d "." -f 1-3)
pkgrel=1
arch=('x86_64')
pkgdesc='Kaspersky Endpoint Security 12.4.0 for Linux'
url='https://support.kaspersky.com/help/KES4Linux/12.4.0/en-US/264264.htm'
license=('custom')
noextract=("kesl_${_pkgver}-${_pkgverbuild}_amd64.deb")
depends=('perl')
options=("!strip")
conflicts=( 'eea'
            'esets'
            'eea-dkms'
            'eea7-dkms')
install="${pkgname}.install"

#https://www.kaspersky.com/small-to-medium-business-security/downloads/endpoint?utm_content=downloads
source=( "https://products.s.kaspersky-labs.com/endpoints/keslinux10/12.4.0.1225/multilanguage-INT-12.4.0.1225/167a2f9c5bdb4e869f4603fbc3a829c1/kesl_12.4.0-1225_amd64.deb"
         "${pkgname}.install"
         "kesl.ini"
         "kesl.start.conf")
sha256sums=('e0befb5dbf628344ecf022259841f9e3688e86b07ce094f45ed871711b98fc7f'
            'c923deff7ddb83552ddd709959f8733754b636ba4406020ea496238a335ec163'
            '72899f7a8d5c63e1541762603cf6fc1a05a9114c60a529e7b18bac2b334040f1'
            '29efcd166bb0fc5baa5a85dc0f41c6c2e253f6b8fd3ee723862496364281cb4c')
validpgpkeys=('6AFE173577C4CBD621DF217FD093435AA3ED2C4A')

package_kesl() {
    # prepare dirs
    mkdir -p ${pkgdir}/usr/src
    mkdir -p ${pkgdir}/etc/init.d

    KESLIDIR=${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver}

    # uncompress base packages
    bsdtar -xf kesl_${_pkgver}-${_pkgverbuild}_amd64.deb
    bsdtar -xf data.tar.xz -C ${pkgdir}/

    # install deb post/pre scripts
    [ ! -d "${pkgdir}/var/opt/kaspersky/kesl/pkgscripts" ] && mkdir -p ${pkgdir}/var/opt/kaspersky/kesl/pkgscripts
    bsdtar -xf control.tar.gz -C ${pkgdir}/var/opt/kaspersky/kesl/pkgscripts
    cp kesl.ini ${pkgdir}/var/opt/kaspersky/kesl/pkgscripts/

    # install licenses
    for lic in $(find ${pkgdir}/var/opt/kaspersky/kesl/install_${pkgver}/opt/kaspersky/kesl/doc/ -maxdepth 1 -mindepth 1 -type f |grep license);do
        install -Dm644 ${lic} "$pkgdir/usr/share/licenses/$pkgname/${lic/*\/}"
    done

    # install startup config
    cp kesl.start.conf ${pkgdir}/var/opt/kaspersky/kesl/pkgscripts/
    sed -i "s/@PKGVER@/$pkgver/g" ${pkgdir}/var/opt/kaspersky/kesl/pkgscripts/kesl.start.conf
}


