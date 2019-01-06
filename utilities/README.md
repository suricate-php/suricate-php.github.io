# Utilities

## Debug helpers

### `_p()` function

`_p()` is used to dump a variable and pretty print it.
It's basically a `print_r()` wrapped into `<pre>` tags

```php
<?php
$myvar = ['a' => 123, 'c' => 456];

_p($myvar)
```

Will output :

```


```

### `_d()` function

## String helpers

### `e()` function

### `camelCase()` function

### `snakeCase()` function

### `contains()` function

### `startsWith()` function

### `endsWith()` function

### `wordLimit()` function

### `slug()` function

## Array helpers

### `head()` function

```php
<?php
head(array $haystack)

```

function

### `last()` function

### `flatten()` function

## Request helpers

### `getParam()` function

getParam() will extract a parameter from the request and return it.
If the parameter is not the in the request, the default value is returned instead.

```php
<?php
$myParam = getParam(string $paramName, $defaultValue = null)
```

### `getPostParam()` function

## Date/Time helpers

### `niceTime()` function
