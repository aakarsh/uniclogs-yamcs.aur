# Maintainer: Dmitri McGuckin <admin@oresat.org>
pkgname=uniclogs-yamcs
pkgver=0.1.0_SNAPSHOT
pkgrel=1
pkgdesc="OreSat UniClOGS Ground Station management software."
arch=('x86_64')
groups=('oresat')
url="https://github.com/oresat/uniclogs-yamcs"
license=('AGPL3')
depends=('java-runtime' 'bash')
makedepends=('maven' 'npm')
replaces=('uniclogs-yamcs')
source=("https://packages.oresat.org/arch/bundles/${pkgname}-${pkgver//_/-}-bundle.tar.gz")
sha256sums=('70e4168e03b5d4584c23b8bacf944cae491f37e809a9f25f93ca10def7050600')

package() {
    _src="${pkgname}-${pkgver//_/-}"
    _install="${pkgdir}/opt/yamcs"

    # Create the yamcs user and group
    #useradd --shell /usr/bin/nologin --user-group --no-create-home --system --home-dir /dev/null yamcs

    # Generate the directories
    install -oyamcs -gyamcs -dm755 ${_install}
    install -oyamcs -gyamcs -dm755 ${_install}/bin
    install -oyamcs -gyamcs -dm755 ${_install}/lib
    install -oyamcs -gyamcs -dm755 ${_install}/log
    install -oroot -groot -dm755 ${pkgdir}/etc/yamcs
    install -oroot -groot -dm755 ${pkgdir}/usr/bin

    # Install stuff
    install -oyamcs -gyamcs -Cm755 ${_src}/bin/* ${_install}/bin
    install -oyamcs -gyamcs -Cm755 ${_src}/lib/* ${_install}/lib
    install -oroot -groot -Cm644 ${_src}/etc/* ${pkgdir}/etc/yamcs
    install -oroot -groot -CDm644 ${_src}/mdb/* -t ${pkgdir}/etc/yamcs/mdb

    # Link binaries
    for file in $(ls ${_install}/bin); do
        echo -e "Linking: $file"
        ln -s /opt/yamcs/bin/$file ${pkgdir}/usr/bin
    done
}
