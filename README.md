# PHP Awful

A curated list of awful PHP language features. Inspired by various languages / frameworks' awesome repository.

## Return Values

### `substr($string, $start[, $length])`

> Returns the portion of string specified by the start and length parameters.

#### What's the problem?

The problem of `substr` is about its return value:

> Returns the extracted part of string;

Looks good! But...

> or **FALSE on failure**, or **an empty string**.

Imagine the case that you need to keep on slicing the string until it is empty (i.e. writing a tokenizer), you would simply do something like this:

```php
while ($str !== '') {
    $str = substr($str, 1);
}
```

The code itself looks so fine, no logical error, and careful enough to use `!==` to compare against empty string.

However, this is an infinite loop because `$str === false` when it slice away the very last character, and it never meets `$str === ''`! **Oh no!**

#### No-no workaround

```php
while (!empty($str)) {
    $str = substr($str, 1);
}
```

What about your very last lovely character in `$str` is `'0'`? You will stop eariler than you want!

```php
while ($str !== false) {
    $str = substr($str, 1);
}
```

This works well! But what are you doing?

#### My Solution

I keep track of the remaining length of the string myself, so that I would slice the string until the length is `0`:

```php
$len = strlen($str);
while ($len > 0) {
    $offset = 1; // determine how many characters to slice away this time

    // ... other stuff

    $str = substr($str, $offset);
    $len -= $offset;
}
```

This is longer to write, but at least I am very sure about what I'm doing.
