# Maintainer: George Rawlinson <grawlinson@archlinux.org>

pkgname=python-rpds-py
pkgver=0.18.1
pkgrel=1
pkgdesc='Python bindings to the Rust rpds crate for persistent data structures'
arch=('x86_64')
url='https://github.com/crate-py/rpds'
license=('MIT')
depends=(
  'glibc'
  'gcc-libs'
  'python'
)
makedepends=(
  'git'
  'python-build'
  'python-maturin'
  'python-installer'
)
source=("$pkgname::git+$url#tag=v$pkgver")
sha512sums=('e02ba2538fb20a1fd10e86e34fa0aae879e105a8c5cad7445967bbd0c745d6586d6cd1fb6fac41432d377e6cc96512a5761e9d07c3783f1cf6e8605ef3bd4c19')
b2sums=('600ba9d00576cffc1991de8b35e8859392a293e192ac4523be89800346b5935151b560ec15e8c1aba812133353546ba3de1701d84a851cb2c0fa79ff74c47a86')

prepare() {
  cd "$pkgname"

  # download dependencies
  cargo fetch --locked --target "$CARCH-unknown-linux-gnu"
}

build() {
  cd "$pkgname"

  python -m build --wheel --no-isolation
}

package() {
  cd "$pkgname"

  python -m installer --destdir="$pkgdir" dist/*.whl

  # symlink license file
  local site_packages=$(python -c "import site; print(site.getsitepackages()[0])")
  install -d "$pkgdir/usr/share/licenses/$pkgname"
  ln -s "$site_packages/rpds_py-$pkgver.dist-info/license_files/LICENSE" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
