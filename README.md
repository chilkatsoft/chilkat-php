# chilkat/chilkat

The [Chilkat](https://www.chilkatsoft.com/) PHP extension as a Composer package: the `CkHttp`, `CkCrypt2`,
`CkEmail`, `CkImap`, `CkMailMan`, `CkZip`, `CkFtp2`, `CkSFtp`, `CkSsh`, `CkRest`, `CkJsonObject`, `CkXml`, `CkPdf`,
`CkCert`, `CkJwt` ... classes (110 in all), plus a command that installs the native extension for your PHP.

```
composer require chilkat/chilkat
vendor/bin/chilkat-install
```

That's it. `chilkat-install` asks the PHP it runs under which version, architecture and thread-safety it is,
downloads the matching build of the extension (`chilkat.so` / `chilkat.dll`) from chilkatdownload.com, verifies
its SHA-256, copies it into PHP's `extension_dir`, enables it in the right place (`phpenmod` on Debian/Ubuntu,
a `chilkat.ini` in the conf.d directory on Alpine, RHEL/Fedora, Homebrew and the official Docker images, or an
`extension=` line in `php.ini` — created next to `php.exe` on Windows when there is none), and verifies that a fresh
PHP loads it. Supported: PHP 7.2 – 8.5 on Linux (x86_64, x86, arm64, armv7l), Alpine Linux (x86_64, arm64),
macOS (Apple silicon, Intel) and Windows (x64, x86; TS and NTS).

The package version is the Chilkat version (`11.6.1` installs Chilkat 11.6.1; a fourth number, e.g. `11.6.1.3`, is a
fix to the package itself and still installs Chilkat 11.6.1), so the classes and the extension always match.
Updating is `composer update chilkat/chilkat` followed by `vendor/bin/chilkat-install` again. Let `composer require`
write its usual caret constraint (`"chilkat/chilkat": "^11.6"`) rather than pinning an exact version, so those
updates come through.

## Usage

```php
<?php
require 'vendor/autoload.php';

$glob = new CkGlobal();
$glob->UnlockBundle('Anything for 30-day trial');   // your unlock code after purchase

$http = new CkHttp();
$html = $http->quickGetStr('https://www.chilkatsoft.com/');
if ($html === null) {
    echo $http->lastErrorText(), "\n";
} else {
    echo strlen($html), " bytes\n";
}
```

The classes are global (no namespace), one per file, loaded on demand through Composer's classmap. Every method
carries a PHPDoc summary and a link to its reference page, so IDEs and static analyzers see signatures, parameter
names and return types without extra stubs. Reference documentation: https://www.chilkatsoft.com/refdoc/,
examples: https://www.example-code.com/php/.

## chilkat-install

```
vendor/bin/chilkat-install [--dry-run] [--no-ini] [--dir=DIR] [--version=X.Y.Z] [--uninstall]
```

* It installs for **the PHP that runs it**. To target a specific PHP (the one PHP-FPM uses, or one of several
  versions) run that binary explicitly: `/usr/bin/php8.2 vendor/bin/chilkat-install`.
* `--dry-run` prints what would be done — which build, where it goes, which ini file — and changes nothing.
* Installing into a system `extension_dir` usually needs root: the script tells you when, and prints the exact
  `sudo` command to run (Windows: an Administrator prompt). `--dir=DIR` installs somewhere writable instead; PHP
  ≥ 7.2 accepts the absolute path the ini line then uses.
* `--no-ini` installs the file and prints the `extension=` line for you to add yourself.
* `--uninstall` removes the extension file and the ini entry it wrote.
* If PHP runs under a web server or PHP-FPM, restart it after installing so it picks up the extension.

Without Composer, the same installer exists as a shell script and a PowerShell script:
`curl -fsSL https://chilkatdownload.com/php/install-php.sh | sudo sh` and
`irm https://chilkatdownload.com/php/install-php.ps1 | iex` — see https://www.chilkatsoft.com/php.asp.

## Docker and CI

In a Dockerfile based on the official `php` images (Alpine or Debian; cli, fpm, apache) the build runs as root and
the extension is enabled through `$PHP_INI_DIR/conf.d` for every SAPI, so:

```Dockerfile
FROM php:8.3-cli-alpine
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer
WORKDIR /app
COPY . .
RUN composer install --no-dev --no-interaction && vendor/bin/chilkat-install
```

Without Composer, `RUN curl -fsSL https://chilkatdownload.com/php/install-php.sh | sh` does the same for the
image's PHP (`| sh -s -- --version 11.6.1` pins the Chilkat version). On Alpine both installers also add the
`libstdc++` package, which the minimal `php:*-alpine` images do not ship and `chilkat.so` needs. In a multi-stage
build, run the installer in the final stage.

GitHub Actions (PHP from `shivammathur/setup-php` keeps a root-owned `extension_dir`, hence `sudo`):

```yaml
- uses: shivammathur/setup-php@v2
  with:
    php-version: '8.3'
- run: composer install --no-interaction
- run: sudo php vendor/bin/chilkat-install
```

## License

Chilkat is commercial software: the extension works in trial mode for 30 days, after which a
[license](https://www.chilkatsoft.com/purchase2.asp) is required. See LICENSE.

## Support

https://www.chilkatforum.com/ · support@chilkatsoft.com
