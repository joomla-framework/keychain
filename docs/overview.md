## Overview

The Keychain package is an extension of the [Registry](https://github.com/joomla-framework/registry) package which provides a way to securely store sensitive information such as access credentials or any other data.

### Creating your Keychain

The `Joomla\Keychain\Keychain` class constructor takes 1 compulsory and 1 optional parameter:

```php
/**
 * Constructor
 *
 * @param   Crypt  $crypt  The encryption handler.
 * @param   mixed  $data   The data to bind to the new Keychain.
 */
public function __construct(Crypt $crypt, $data = null)
```

Please review the documentation for the [Crypt](https://github.com/joomla-framework/crypt) package for information about configuring a `Joomla\Crypt\Crypt` instance.

Once created, the `Keychain` class inherits the `Joomla\Registry\Registry` class' API with added methods for loading a file previously saved by the `Keychain` class and saving the data from a `Keychain` as an encrypted file.

### Loading an encrypted file

To load a previously saved keychain, the `loadKeychain()` method should be called, specifying the path to the file to be loaded.

```php
<?php
use Joomla\Crypt\Crypt;
use Joomla\Keychain\Keychain;

// You should configure your own `Joomla\Crypt\CipherInterface` and `Joomla\Crypt\Key` objects for proper re-use, however this example will create a valid `Joomla\Crypt\Crypt` instance
$crypt = new Crypt;

$keychain = new Keychain($crypt);
$keychain->loadKeychain('/path/to/keychain.file');
```

### Saving an encrypted file

To save the data from your keychain, the `saveKeychain()` method should be called, specifying the path for the file to be created.

```php
<?php
use Joomla\Crypt\Crypt;
use Joomla\Keychain\Keychain;

// You should configure your own `Joomla\Crypt\CipherInterface` and `Joomla\Crypt\Key` objects for proper re-use, however this example will create a valid `Joomla\Crypt\Crypt` instance
$crypt = new Crypt;

$keychain = new Keychain($crypt);
$keychain->set('foo', 'bar');
$keychain->saveKeychain('/path/to/keychain.file');
```

### Command line management

A keychain can optionally be managed if your application integrates the [Console](https://github.com/joomla-framework/console) package, please review the [command line management](features/command-line-management.md) page for more information.

## Things to know before you build on this

**The keychain file is written with default permissions.** `saveKeychain()` calls
`file_put_contents()` without adjusting the mode, so with a typical umask the file ends up
world-readable. Tighten it yourself after saving:

```php
$keychain->saveKeychain($file);
chmod($file, 0600);
```

**Writes are not atomic and not locked.** A crash mid-write leaves an unreadable keychain, and two
processes saving at once will lose one set of changes. If that matters, write to a temporary file
and `rename()` it into place.

**Load errors are not reported.** `loadKeychain()` checks that the file exists but not that it
could be read, and does not check the result of `json_decode()`. With a cipher that does not
authenticate its ciphertext, a corrupted or tampered file decrypts to noise and yields an empty
keychain rather than an error — so a missing entry can mean "not set" or "file damaged".

```php
$keychain->loadKeychain($file);

if ($keychain->get('expected-key') === null) {
    // Cannot tell a missing entry from a damaged file. Check explicitly if it matters.
}
```

**The class extends `Registry`.** That brings `loadFile()`, `loadString()`, `toString()`,
`__toString()` and `jsonSerialize()` along with it — all of which read or write **unencrypted**
data. A stray `json_encode($keychain)` or string interpolation writes every secret in clear text.
Treat the object as write-only outside of `get()`.

**Secrets on the command line are visible to other users.** `keychain:add-entry` and
`keychain:edit-entry` take the value as an argument, and command lines are readable through `ps`
and land in the shell history. Prefer piping the value in, or set it from PHP.

**There is no `keychain:create`.** Every command requires the file to exist already, so the first
entry cannot be added through the CLI. Create the file from PHP once:

```php
(new Keychain($crypt))->saveKeychain('/path/to/keychain.dat');
```
