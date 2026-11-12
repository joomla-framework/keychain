# Joomla Keychain Package — Documentation

An encrypted key/value store built on `joomla/registry` and `joomla/crypt`, with console commands
for managing it.

> **This package has moved.** Its contents live in `joomla/registry` from registry 4.1 onwards,
> where the keychain is an optional feature. The pages below describe the last standalone release;
> for anything new, see the
> [keychain documentation in the registry package](https://github.com/joomla-framework/registry/blob/4.x-dev/docs/keychain.md),
> which also lists how the class names map across.

## Guide

* [Overview](overview.md) — creating, loading and saving a keychain
* [Keychain class](classes/Keychain.md) — the API in detail
* [Command line management](features/command-line-management.md) — the `keychain:*` commands

## Upgrading

* [Updating from v1 to v2](v1-to-v2-update.md) — `KeychainManager` removed, `joomla/crypt` introduced
* [Updating from v2 to v3](v2-to-v3-update.md) — PHP 8.1, PSR-12, no API changes
* [Updating from v3 to v4](v3-to-v4-update.md) — PHP 8.3, no API changes
