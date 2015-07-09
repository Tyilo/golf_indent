# golf_indent

Utility for indenting a Python 2 file using fewest possible bytes.

Usage
==

You can provide the file that should be re-indented to `golf_indent` either via stdin or by supplying a filename as the first argument:

```
$ golf_indent < script.py
...
$ golf_indent script.py
...
```

Example
==

`^I` is the tab character and `^D` is ctrl-D.

```
$ cat > test1.py
0
	1
		2
			3
			3
			3
^D
$ golf_indent test1.py | cat -t
Saved 8 bytes.
Indentation used:
  1: 1 space
  2: 2 spaces
  3: 1 tab
0
 1
  2
^I3
^I3
^I3
$ cat > test2.py
0
	1
		2
		2
		2
			3
^D
$ golf_indent test2.py | cat -t
Saved 4 bytes.
Indentation used:
  1: 1 space
  2: 1 tab
  3: 1 tab and 1 space
0
 1
^I2
^I2
^I2
^I 3
```

How it works
==

In Python 2 a tab character counts as 8 spaces for indentation, but is only 1 byte. We can exploit this to reduce the number of bytes needed for indentation in a Python 2 file.

The optimal way to indent code with 3 or fewer indentation levels is always:

```
1: space
2: tab
```

However for code with more indentation levels, it depends on the number of lines for each indentation, which indentation is optimal.

For instance, when the code has 4 indentation levels, there are two possible optimal indentations:

```
1: space
2: 2 spaces
3: tab
```

or

```
1: space
2: tab
3: tab + space
```

The script tries all possible optimal indentations for a file and outputs the file with that indentation applied. The script also prints the number of bytes saved and the indetation used to stderr.
