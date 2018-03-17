pkgname=freetube
_pkgfullname=FreeTube
pkgver=0.1.3.80
_pkgfullver=0.1.3-beta
pkgrel=1
pkgdesc="An open source youtube app for privacy"
url="https://github.com/${_pkgfullname}App/${_pkgfullname}"
arch=('x86_64')
license=('GPL')
depends=(
        'atk'
        'avahi'
        'cairo'
        'e2fsprogs'
        'gconf'
        'gdk-pixbuf2'
        'gtk2'
        'keyutils'
        'krb5'
        'libcups'
        'libdatrie'
        'libthai'
        'libxinerama'
        'pango'
        'pixman'
)
makedepends=(
    'findutils'
    'nodejs'
)
source=(
    "${pkgname}.tar.gz::${url}/archive/v${_pkgfullver}.tar.gz"
    'kaos.patch'
    'freetube.png'
    'freetube.desktop'
)
sha1sums=(
    '8c6bee6bd4f2138c1f225ee552571d9f86561dc0'
    '7a0bd67bd3093c66836e75d25b187938f43cd389'
    'c90c0e0b5afd5bec0070044e7c6fa097c1ebf660'
    '7c0561404fac2fca774c69447442a00f60e7240a'
)

prepare() {
    cd "${srcdir}/${_pkgfullname}-${_pkgfullver}"
    # add make script for non-deb linux distro
    patch -Np1 -i "${srcdir}/kaos.patch"
    npm install --only=prod
    npm install --only=dev
}

build() {
    cd "${srcdir}/${_pkgfullname}-${_pkgfullver}"
    npm run make:kaos
}

package() {
    outdir="${srcdir}/${_pkgfullname}-${_pkgfullver}/out/${pkgname}-linux-x64"
    cd "${outdir}"

    srcfiles=($(find "./" -type f -printf "%m:%p\n"))
    
    for f in "${srcfiles[@]}"; do
        # strip permissions and : delimiter
        fpath="${f#*:}"
        # strip : delimiter and file path
        srcperms="${f%:*}"
        # copy source files to pkg dir, making dirs as needed and
        # retaining original perms.
        install -Dm ${srcperms} "${outdir}/${fpath}" "${pkgdir}/opt/${pkgname}/${fpath}"
    done
    
    install -Dm 644 "${srcdir}/freetube.png" "${pkgdir}/usr/share/icons/hicolor/48x48/apps/freetube.png"
    install -Dm 644 "${srcdir}/freetube.desktop" "${pkgdir}/usr/share/applications/freetube.desktop"
    install -dm 755 "${pkgdir}/usr/bin"
    ln -s "/opt/${pkgname}/${pkgname}" "${pkgdir}/usr/bin/${pkgname}"
}
