# Maintainer: Mads Mogensen <mads256h at gmail dot com>

pkgname=centralhub-git
pkgver=9f3ac3b
pkgrel=1
pkgdesc="Central hub stuff"
arch=('x86_64' 'armv7h')
url=https://github.com/cs-23-sw-7-06/CentralHub
license=('gpl-3')
depends=('dotnet-sdk' 'aspnet-runtime')
source=("$pkgname::git+https://github.com/cs-23-sw-7-06/CentralHub.git" "centralhub-api.service" "centralhub-webui.service")
sha256sums=('SKIP'
            '925e4b9bcc51d512faf5f23596047ebcfb965aa9a9284e0ad08f0b7af48540b3'
            'fe829b5b5ee35de4d967cfe0c923391d79e4931c80af36f2ef5026af8213a227')

pkgver() {
	cd "$srcdir/$pkgname/"
	git rev-parse --short HEAD
}

build() {
  cd "$srcdir/$pkgname/"
  dotnet restore

# Build CentralHub.Api
  dotnet publish CentralHub.Api -c "Release" -o "${srcdir}/api/"

# Build CentralHub.WebUI
  dotnet publish CentralHub.WebUI -c "Release" -o "${srcdir}/webui/"
}

check() {
  cd "$srcdir/$pkgname"
  dotnet test
}

package () {
# Copy programs to /opt/centralhub/
  mkdir -p "${pkgdir}/opt/centralhub/"
  cp -r -a "${srcdir}/api/" "${pkgdir}/opt/centralhub/api"
  cp -r -a "${srcdir}/webui/" "${pkgdir}/opt/centralhub/webui"

# Make programs available in path
  mkdir -p "${pkgdir}/usr/bin/"
  ln -s "/opt/centralhub/api/CentralHub.Api" "${pkgdir}/usr/bin/centralhub-api"
  ln -s "/opt/centralhub/webui/CentralHub.WebUI" "${pkgdir}/usr/bin/centralhub-webui"

# Systemd services
  install -D -m644 "${srcdir}/centralhub-api.service" "${pkgdir}/usr/lib/systemd/system/centralhub-api.service"
  install -D -m644 "${srcdir}/centralhub-webui.service" "${pkgdir}/usr/lib/systemd/system/centralhub-webui.service"
}

