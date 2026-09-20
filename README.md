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

The package version is the Chilkat version (`11.6.1` installs Chilkat 11.6.1), so the classes and the extension
always match. Updating is `composer update chilkat/chilkat` followed by `vendor/bin/chilkat-install` again.

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
* Docker: `RUN composer require chilkat/chilkat && vendor/bin/chilkat-install` works as-is in the official
  `php:*` images (they have the conf.d directory and run as root during the build).

Without Composer, the same installer exists as a shell script and a PowerShell script:
`curl -fsSL https://chilkatdownload.com/php/install-php.sh | sudo sh` and
`irm https://chilkatdownload.com/php/install-php.ps1 | iex` — see https://www.chilkatsoft.com/php.asp.

## License

Chilkat is commercial software: the extension works in trial mode for 30 days, after which a
[license](https://www.chilkatsoft.com/purchase2.asp) is required. See LICENSE.

## Support

https://www.chilkatforum.com/ · support@chilkatsoft.com
