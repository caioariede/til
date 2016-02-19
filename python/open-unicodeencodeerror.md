Python 2.x will not handle unicode filenames correctly if `LC_ALL` variable
points to a non-unicode encoding:

```
$ LC_ALL= python -c "open(u'\u4e09')"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  UnicodeEncodeError: 'ascii' codec can't encode character u'\u4e09' in position 0: ordinal not in range(128)
```

**Correct:**

```
$ LC_ALL=en_US.UTF-8 python -c "open(u'\u4e09')"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
IOError: [Errno 2] No such file or directory: u'\u4e09'
```
