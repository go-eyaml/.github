<p align="center"><img src="https://raw.githubusercontent.com/go-eyaml/brand/main/social/go-eyaml.png" alt="go-eyaml" width="640"></p>

<h1 align="center">go-eyaml</h1>
<p align="center"><strong>eyaml encryption in pure Go — PKCS7 and GPG encrypt/decrypt for hiera-eyaml data, CGO-free.</strong></p>

<p align="center">
  🌐 <a href="https://go-eyaml.github.io">Website</a> ·
  📚 <a href="https://go-eyaml.github.io/docs/">Documentation</a>
</p>

<p align="center">
  <a href="https://go-eyaml.github.io/docs/"><img alt="Docs" src="https://img.shields.io/badge/docs-mkdocs--material-4F46E5?style=flat-square"></a>
  <a href="https://github.com/go-eyaml/eyaml/blob/main/LICENSE"><img alt="License: BSD-3-Clause" src="https://img.shields.io/badge/license-BSD--3--Clause-blue?style=flat-square"></a>
  <img alt="Go 1.26.4+" src="https://img.shields.io/badge/go-1.26.4%2B-00ADD8?style=flat-square&logo=go&logoColor=white">
  <img alt="Coverage 100%" src="https://img.shields.io/badge/coverage-100%25-1a7f37?style=flat-square">
</p>

---

go-eyaml is a pure-Go (CGO_ENABLED=0) implementation of the two encryption schemes used by Puppet's hiera-eyaml: the ENC[PKCS7,<base64>] and ENC[GPG,<base64>] token formats that carry an encrypted value inside otherwise-plaintext YAML. For PKCS7, a random 256-bit AES content key encrypts the plaintext with AES-256-CBC, the content key is wrapped for the recipient with RSA PKCS#1 v1.5 under an X.509 certificate, and the whole thing is serialised as a CMS EnvelopedData (RFC 5652) token — built exclusively on the standard library's crypto packages. For GPG, a session key is public-key-encrypted to recipient OpenPGP keys exactly as `gpg --encrypt` does, using the pure-Go `ProtonMail/go-crypto/openpgp` implementation. Both stay CGO_ENABLED=0. 100% coverage, six arches and WebAssembly.

## Repositories

| Repo | What it is |
|------|------------|
| [**eyaml**](https://github.com/go-eyaml/eyaml) | the engine library |
| [**docs**](https://github.com/go-eyaml/docs) | MkDocs Material documentation, versioned with [mike], served at [/docs/](https://go-eyaml.github.io/docs/) |
| [**go-eyaml.github.io**](https://github.com/go-eyaml/go-eyaml.github.io) | the Hugo landing page |
| [**brand**](https://github.com/go-eyaml/brand) | logos and brand assets |

## Principles

- **Pure Go, zero cgo.** `CGO_ENABLED=0`; PKCS7 imports the Go standard library's crypto packages only, GPG adds the
  pure-Go `ProtonMail/go-crypto/openpgp`. Cross-compiles to the
  six 64-bit Go targets (amd64, arm64, riscv64, loong64, ppc64le, s390x) and WebAssembly, linking into a static binary.
- **Interoperable with hiera-eyaml's pkcs7 and gpg tokens.**
- **An engine, not a service.** A small, stable Go API you embed — part of the
  pure-Go Puppet stack (siblings [go-facter](https://github.com/go-facter),
  [go-hiera](https://github.com/go-hiera), [go-pcore](https://github.com/go-pcore),
  [go-puppet](https://github.com/go-puppet)).
- **100% test coverage** including error branches, enforced as a CI gate.

BSD-3-Clause.

[mike]: https://github.com/jimporter/mike
