# Vim Tricks that Blew My Mind
This is a list of Vim tricks that made me say WHATTTTTTTT. This list be kept updated.

- [Auto Find for `ci`](#auto-find-for-ci)
- [Exit with `ZZ`](#exit-with-zz)
- [Fix Indentation with `=i`](#fix-indentation-with-i)
- [Line Wrap Navigation with `g`](#line-wrap-navigation-with-g)

## Auto Find for `ci`
You have a line of code with an IP between quotes and you want to change the IP:
```cpp
some_function("http://127.0.0.1:9999");
```
Assume cursor is at start of line. Sane human beings will use `f"ci"`. But you can also directly do `ci"` and Vim will jump into quote automatically. This also works with `di`/`yi` and similar.

## Exit with `ZZ`
In normal mode, `ZZ` is equivalent to `:x` (or `:wq`) and `ZQ` is equivalent to `:q!`.

## Fix Indentation with `=i`
You have some messed up indentation:
```cpp
if (some_function()) {
another_function();
if (number > 0) {
number = 0;
}
}
```
In normal mode, place cursor between curly braces and use `=i{` to watch Vim fix your indentation.

## Line Wrap Navigation with `g`
You have a very long line and it is line wrapped in Vim. Using `j`/`k` will skip all the line wrap and jump to next/previous "real" line. Using `gj`/`gk` will operate on the "fake" line wrap lines. End of line/start of line will also work on "fake" lines using `g$`/`g0`.
