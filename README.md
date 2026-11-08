Maintained by [Ankara Yazılım](https://github.com/Ankara-Software) | [Project page](https://ankarayazilim.org/acik-kaynak/rustenv)

**Status: beta.** It builds, the 5 unit tests pass and every command below was run by hand. Encryption derives its key from the password with a single unsalted SHA-256, so use long random passwords, and test coverage is small (see [Encryption](#encryption)).

<div align="center">

<h1>rustenv</h1>

<p><strong>A command line tool to inspect, compare, validate, export and encrypt <code>.env</code> files — written in Rust.</strong></p>

[![License: MIT](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](LICENSE)
[![GitHub Release](https://img.shields.io/github/v/release/Ankara-Software/rustenv?style=flat-square&color=8be04e)](https://github.com/Ankara-Software/rustenv/releases)

```bash
rustenv show          # view .env with masked secrets
rustenv diff .env .env.production   # compare two env files
rustenv encrypt .env  # AES-256-GCM encrypted .env.enc
```

</div>

## Türkçe

`.env` dosyalarını incelemek, karşılaştırmak, doğrulamak, dışa aktarmak ve şifrelemek için yazılmış bir komut satırı aracıdır. Gizli değerleri (anahtar, parola, token vb.) varsayılan olarak maskeleyerek gösterir. İki `.env` dosyasını karşılaştırır, iki dosyayı birleştirir, şablondan `.env` üretir ve dosyayı AES-256-GCM ile `.env.enc` olarak şifreler. Proje beta aşamasındadır: şifreleme anahtarı paroladan tek bir tuzsuz SHA-256 ile türetilir, bu yüzden uzun ve rastgele parola kullanın.

---

## Installation

### Install script (Linux / macOS)
Downloads the release binary for your platform and installs it as `rustenv`:
```bash
curl -fsSL https://raw.githubusercontent.com/Ankara-Software/rustenv/master/install.sh | sh
```

### Cargo (Rust 1.85+)
```bash
cargo install --git https://github.com/Ankara-Software/rustenv
```
The crate is also published on crates.io as `rustenv-cli` (`cargo install rustenv-cli`). The published 0.1.1 installs a binary named `rustenv-cli`; building from this repository installs `rustenv`. The crates.io name `rustenv` belongs to a different project.

---

## Features

- **Show** — print a `.env` file; values of keys that look secret (key, secret, password, token, auth, pass, private, cred) are masked unless `--unmask` is given
- **Diff** — compare two `.env` files and list added, removed and changed keys
- **Validate** — flag empty values, key names that are not upper case with underscores, `*_URL` values that do not start with http(s)://, `*_EMAIL` values that are not email-like, `*_PORT` values that are not a number from 0 to 65535, and boolean-style keys (`*_BOOL`, `ENABLE_*`, `DISABLE_*`) that are not true/false/1/0. Exits with code 1 when there are errors
- **Export** — print variables as `default`, `shell`, `docker` or `json`; secrets are masked unless `--unmask` is given
- **Merge** — apply the values of a second file over a base file, to stdout or `--output`
- **Init** — create a `.env` from a `.env.template`, optionally prompting for each value with `--interactive`
- **Encrypt / Decrypt** — AES-256-GCM encrypt a file into `.env.enc` and decrypt it back, with `--password` or an interactive prompt

Not implemented: duplicate key detection (the parser keeps the last value of a repeated key) and variable interpolation.

## Usage

```bash
rustenv show .env --unmask
rustenv diff .env .env.production
rustenv validate .env
rustenv export .env --format json --unmask
rustenv merge .env .env.production --output .env.merged
rustenv encrypt .env --password "long random password"
rustenv decrypt .env.enc --output .env --password "long random password"
```

If the input file does not exist, `show`, `diff`, `validate` and `export` treat it as empty instead of failing.

## Encryption

The key is the SHA-256 hash of the password with no salt and no key stretching, and the file format is a 12 byte random nonce followed by the AES-256-GCM ciphertext. This protects against casual reading but a weak password can be brute forced quickly. Do not rely on it for high value secrets.

## Development

```bash
cargo build
cargo test
```

See [CONTRIBUTING.md](CONTRIBUTING.md), [SECURITY.md](SECURITY.md) and [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

## License

MIT. Copyright (c) 2026 Anilcan Kara. See [LICENSE](LICENSE).
