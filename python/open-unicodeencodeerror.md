## Python open() raises UnicodeEncodeError when using unicode filename with non-unicode filesystem.

Python won't handle unicode filenames correctly if the filesystem encoding isn't set to unicode, like this:

```bash
$ python -c 'import sys; print(sys.getfilesystemencoding())'
utf-8
```

According to the [Python 2 documentation](https://docs.python.org/2/howto/unicode.html#unicode-filenames), this won't affect Mac OS X that always use UTF-8, but will affect other Unix and Linux. On **Linux**, `ASCII` is the default encoding for **Python 2** but can be overridden by `LC_ALL`, `LC_CTYPE` or `LANG` environment variables. According to the [Python 3 documentation](https://docs.python.org/3.3/howto/unicode.html#unicode-filenames), its default filesystem encoding is `UTF-8`.

```bash
$ LANG= LC_CTYPE= LC_ALL= python -c 'import sys; print(sys.getfilesystemencoding())'
ANSI_X3.4-1968
```

```bash
$ LANG=en_US.UTF-8 LC_CTYPE= LC_ALL= python -c 'import sys; print(sys.getfilesystemencoding())'
UTF-8
```

If the filesystem encoding is not set to a unicode encoding, the following will raise `UnicodeEncodeError`:

```bash
$ LANG= LC_CTYPE= LC_ALL= python -c "open(u'unícódé')"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 2-3: ordinal not in range(128)
```

```bash
$ LANG=en_US.UTF-8 LC_CTYPE= LC_ALL= python -c "open(u'unícódé')"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
IOError: [Errno 2] No such file or directory: u'un\xc3\xadc\xc3\xb3d\xc3\xa9'
```
