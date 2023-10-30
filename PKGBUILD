# Maintainer: Mads Mogensen <mads256h at gmail dot com>

pkgname=centralhub-git
pkgver=8b53787
pkgrel=1
pkgdesc="Central hub stuff"
arch=('x86_64' 'armv7h')
url=https://github.com/cs-23-sw-7-06/CentralHub
license=('gpl-3')
depends=('dotnet-sdk' 'aspnet-runtime')
source=("$pkgname::git+https://github.com/cs-23-sw-7-06/CentralHub.git" "centralhub-api.service" "centralhub-webui.service")
sha256sums=('SKIP'
            '7ba87e257f199de375913e2553cbd95599522a8876cda696a81c70d2547f5629'
            '05b16c5915f0f5ae2e84de4d4e3e948244aa1a12a3463a67e5d6b569e5f75c6c')

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

