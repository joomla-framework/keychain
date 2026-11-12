# Updating from v3 to v4

Release 4.0.0 raises the PHP requirement. Nothing else changed in `src/`.

## At a glance

| | v3 (3.0.1) | v4 (4.0.0) |
|---|---|---|
| PHP | `^8.1.0` | `^8.3.0` |
| Public API | — | unchanged |

## Minimum supported PHP version raised

All Framework packages now require **PHP 8.3** or newer.

## No API changes

`git diff 3.0.1 HEAD -- src/` is empty. `Keychain` and the five console commands are byte-for-byte
identical to 3.0.1.

## Dependency changes

| Package | v3 (3.0.1) | v4 (4.0.0) |
|---|---|---|
| `php` | `^8.1.0` | `^8.3.0` |
| `joomla/crypt` | `^3.0` | `^4.0` |
| `joomla/registry` | `^3.0` | `^4.0` |

`joomla/console` remains optional and moved to `^4.0`.

## Worth doing while you are here

If your keychain was set up following the v1 to v2 migration example, it uses
`Joomla\Crypt\Cipher\OpenSSL`. That cipher reuses a single initialisation vector for every value
and does not authenticate the ciphertext. Moving to `Cipher\Crypto` means re-encrypting the
keychain, so it is a deliberate step rather than a drop-in change:

```php
use Joomla\Crypt\Cipher\Crypto;
use Joomla\Crypt\Crypt;
use Joomla\Keychain\Keychain;

// Read with the old cipher …
$old = new Keychain(new Crypt($oldCipher, $oldKey));
$old->loadKeychain('/path/to/keychain.dat');

// … and write with the new one.
$cipher = new Crypto();
$key    = $cipher->generateKey();      // store $key->getPrivate() somewhere safe

$new = new Keychain(new Crypt($cipher, $key), $old->toArray());
$new->saveKeychain('/path/to/keychain.dat');
```
