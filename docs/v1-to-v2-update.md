## Updating from v1 to v2

The following changes were made to the Keychain package between v1 and v2.

### Minimum supported PHP version raised

All Framework packages now require PHP 7.0 or newer.

### Joomla\Keychain\KeychainManager class removed

The `KeychainManager` class has been removed without replacement, this was the command line utility for managing keychains.

### Dependent upon `joomla/crypt`

In v1 the Keychain had a hardcoded dependency to PHP's `ext/openssl`.  The Keychain has been refactored to use the `joomla/crypt`
package to handle the encryption and decryption of data and now allows use of other encryption systems to manage data.

#### Migrating to v2

To use the v2 package, the `Joomla\Keychain\Keychain` must be instantiated with a `Joomla\Crypt\Crypt` object.  The below
code snippet demonstrates how to instantiate the keychain with a configuration similar to the v1 API.

```php
<?php
use Joomla\Crypt\Cipher\OpenSSL;
use Joomla\Crypt\Crypt;
use Joomla\Keychain\Keychain;

$cipher = new OpenSSL('1234567890123456', 'aes-128-cbc');
$key    = $cipher->generateKey(['passphrase' => '/path/to/passphrase.file']);

$crypt = new Crypt($cipher, $key);

$keychain = new Keychain($crypt);
```