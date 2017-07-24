# Maintainer: Harsha Kuchampudi <harshakuchampudi@gmail.com>
pkgname=windscribe-cli
pkgver=1.0
pkgrel=1
pkgdesc="Binaries for Windscribe command line interface"
arch=('any')
url="https://windscribe.com/"
depends=('openvpn') 
optdepends=('openvpn: establishing vpn connection')
source=("${pkgname}_${pkgver}_all::https://windscribe.com/install/desktop/linux_deb")
sha256sums=('9b173b1548c04524861839ff6c2e49a2a0e6143e7438cf5f5a784301010d15d1')
install="windscribe-cli.install"

package() {

  echo "Extracting source code..."
  ar xv "${srcdir}/${pkgname}_${pkgver}_all"
  mkdir "${srcdir}/data"
  mkdir "${srcdir}/control"
  tar xf "data.tar.xz" -C "${srcdir}/data"
  tar xf "control.tar.gz" -C "${srcdir}/control"
  echo "Installing bash completion..."
  mkdir -p "${pkgdir}/etc/windscribe/"
  mkdir -p "${pkgdir}/etc/bash_completion.d/"
  install -m 0755 -D "${srcdir}/data/etc/bash_completion.d/windscribe_complete" "${pkgdir}/etc/bash_completion.d/"

  echo "Creating windscribe systemd service..."
  echo "
  [Unit]
  Description=Windscribe VPN
  After=syslog.target network.target remote-fs.target nss-lookup.target
  
  [Service]
  Type=forking
  ExecStart=/usr/bin/windscribe start
  PIDFile=/etc/windscribe/windscribe.pid
  
  [Install]
  WantedBy=multi-user.target" >> "${srcdir}/windscribe.service"
  
  echo "Installing windscribe systemd service..."
  mkdir -p "${pkgdir}/etc/systemd/system/"
  install -m 0644 -D "${srcdir}/windscribe.service" "${pkgdir}/etc/systemd/system/"
  # systemctl daemon-reload
  echo "Installing windscribe binary..."
  mkdir -p "${pkgdir}/usr/bin/"
  install -m 0755 -D "${srcdir}/data/usr/bin/windscribe" "${pkgdir}/usr/bin/"
  chmod +x "${pkgdir}/usr/bin/windscribe"
}