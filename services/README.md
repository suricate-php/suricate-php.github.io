# Services

## Error

## Cache

##### File

To configure Memcache cache handle, just add following entries to your configuration:

```
[Cache]
type = 'file'

[CacheFile]
path = '/var/www/my/cache/directory' // Cache path on filesystem
defaultExpiry = 3600                 // Key default expiration in second
```

Parameters `host`, `port` and `defaultExpiry` are optional.

```php
<?php
$redis = \Suricate\Suricate::CacheFile();
$redis->set("keyname", "value");
```

will set `value` into `keyname` for the default expiry.

##### APC

##### Memcache

To configure Memcache cache handle, just add following entries to your configuration:

```
[Cache]
type = 'memcache'

[CacheMemcache]
host = 'localhost'     // Memcache hostname
port = 11211          // Memcache default port
defaultExpiry = 3600 // Key default expiration in second
```

Parameters `host`, `port` and `defaultExpiry` are optional.

```php
<?php
$redis = \Suricate\Suricate::CacheMemcache();
$redis->set("keyname", "value");
```

##### Redis

Redis cache is based on [Predis client](https://github.com/nrk/predis), and thus must be installed as a dependency of your project.

To configure Redis cache handle, just add following entries to your configuration:

```
[Cache]
type = 'redis'

[CacheRedis]
host = 'redis'       // Redis hostname
port = 6379          // Redis default port
defaultExpiry = 3600 // Key default expiration in second
```

Parameters `host`, `port` and `defaultExpiry` are optional.

```php
<?php
$redis = \Suricate\Suricate::CacheRedis();
$redis->set("keyname", "value");
```

will set `value` into `keyname` for the default expiry.
If you want to override key expiration value, just add a third parameter :

```php
<?php
$redis = \Suricate\Suricate::CacheRedis();
$redis->set("keyname", "value", 60);
```

Above key will expire 60 seconds after being set. For permanent key (ie without expiration), set expiration value to `-1`

Available methods :

- `set($keyName, $value, [$expiry]): bool`
- `get($keyName): string`
- `delete($keyName): bool`

## Session

##### Cookies

##### Memcache

##### Native

## Curl

## I18n

## Logger
