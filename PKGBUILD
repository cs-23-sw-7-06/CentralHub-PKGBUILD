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
            '8b698d9e02ce7be6df7cd59a2b0d1da90911c9b2ccf73d4f06dd87a9aa9beb9e'
            '1aaf0d570e37b6002db44ab9c138804ef46cd0b06f2fd19035f1dcbf05d33be7')

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

