# Maintainer: Dmitri McGuckin <admin@oresat.org>
pkgname=uniclogs-yamcs
pkgver=0.2.1
pkgrel=1
pkgdesc="OreSat UniClOGS Ground Station management software."
arch=('x86_64')
groups=('oresat')
url="https://github.com/oresat/uniclogs-yamcs"
license=('AGPL3')
depends=('java-runtime' 'bash')
replaces=('uniclogs-yamcs')
source=("https://packages.oresat.org/arch/bundles/${pkgname}-${pkgver//_/-}-bundle.tar.gz")
sha256sums=('5f8afce0c7760d2169569cc100ed6d9b62302a945b1aa00180e021825f9d6c55')

package() {
    _src="${pkgname}-${pkgver//_/-}"
    _static="../static"

    # Create the yamcs user and group
    install -Dm644 ../yamcs.conf "${pkgdir}/usr/lib/sysusers.d/yamcs.conf"

    # Create the systemd service
    install -Dm644 ../yamcs.service "${pkgdir}/usr/lib/systemd/system/yamcs.service"

    # Generate the directories
    install -oroot -groot -dm755 ${pkgdir}/etc/yamcs
    install -oyamcs -gyamcs -dm755 ${pkgdir}/srv/yamcs
    install -oyamcs -gyamcs -dm755 ${pkgdir}/var/log/yamcs
    install -oyamcs -gyamcs -dm755 ${pkgdir}/opt/yamcs/bin
    install -oyamcs -gyamcs -dm755 ${pkgdir}/opt/yamcs/lib
    install -oyamcs -gyamcs -dm755 ${pkgdir}/opt/yamcs/mdb

    # Install stuff
    install -oroot -groot -Cm755 ${_src}/etc/* ${pkgdir}/etc/yamcs
    install -oyamcs -gyamcs -Cm755 ${_src}/bin/* ${pkgdir}/opt/yamcs/bin
    install -oyamcs -gyamcs -Cm755 ${_src}/lib/* ${pkgdir}/opt/yamcs/lib
    install -oyamcs -gyamcs -Cm755 ${_src}/mdb/* ${pkgdir}/opt/yamcs/mdb

    # Install everything in static
    cd ${_static}
    for file in $(find . -type f); do
        SRC=$file
        DEST="${pkgdir}${SRC:1}"
        install -oroot -groot -Cm644 $SRC $DEST
    done
    cd -
}

post_install() {
    # Link stuff
    ln -s /opt/yamcs/bin/yamcsd /usr/bin/yamcsd
    ln -s /opt/yamcs/bin/yamcsadmin /usr/bin/yamcsadmin

    # Enable the service
    systemctl enable --now yamcs
}
