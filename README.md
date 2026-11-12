## The Keychain Package [![Build Status](https://github.com/joomla-framework/keychain/actions/workflows/ci.yml/badge.svg?branch=3.x-dev)](https://github.com/joomla-framework/keychain)

[![Latest Stable Version](https://poser.pugx.org/joomla/keychain/v/stable)](https://packagist.org/packages/joomla/keychain)
[![Total Downloads](https://poser.pugx.org/joomla/keychain/downloads)](https://packagist.org/packages/joomla/keychain)
[![Latest Unstable Version](https://poser.pugx.org/joomla/keychain/v/unstable)](https://packagist.org/packages/joomla/keychain)
[![License](https://poser.pugx.org/joomla/keychain/license)](https://packagist.org/packages/joomla/keychain)

> ## This package has moved
>
> The Keychain package is no longer maintained on its own. Its contents were folded into the
> [Registry](https://github.com/joomla-framework/registry) package in registry 4.1, where the
> keychain is an optional feature.
>
> ```sh
> composer remove joomla/keychain
> composer require joomla/registry joomla/crypt
> ```
>
> The classes are unchanged apart from their namespace — `Joomla\Keychain\Keychain` becomes
> `Joomla\Registry\Keychain`, and `Joomla\Keychain\Command\*` becomes `Joomla\Registry\Command\*`.
> Method signatures, command names and the on-disk file format are the same, so an existing
> keychain file keeps working. See the
> [keychain documentation](https://github.com/joomla-framework/registry/blob/4.x-dev/docs/keychain.md)
> for the full mapping.

The Keychain package is an extension of the [Registry](https://github.com/joomla-framework/registry) package which provides a way to securely store sensitive information such as access credentials or any other data.

## Requirements

* PHP 8.3 or later
* Joomla! Framework [Crypt](https://github.com/joomla-framework/crypt) package
* Joomla! Framework [Registry](https://github.com/joomla-framework/registry) package

## Installation

To install the package, you can run the following from the command line:

```sh
composer require joomla/keychain "~4.0"
```

If you want to include the test sources and docs, use

```sh
composer require --prefer-source joomla/keychain "~4.0"
```
