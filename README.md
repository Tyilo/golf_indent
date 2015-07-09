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

How it works
==

In Python 2 a tab character counts as 8 spaces for indentation, but is only 1 byte. We can exploit this to reduce the number of bytes needed for indentation in a Python 2 file.

The optimal way to indent code with 3 or fewer indentation levels is always:

```
0: <empty>
1: space
2: tab
```

However for code with more indentation levels, it depends on the number of lines for each indentation, which indentation is optimal.

For instance, when the code has 4 indentation levels, there are two possible optimal indentations:

```
0: <empty>
1: space
2: 2 spaces
3: tab
```

or

```
0: <empty>
1: space
2: tab
3: tab + space
```

The script tries all possible optimal indentations for a file and outputs the file with that indentation applied. The script also prints the number of bytes saved to stderr.