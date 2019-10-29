nbconvert-tools
===============
https://github.com/kopptr/notebook-tools


This repo contains scripts to convert between Jupyter notebooks and Python
scripts.

Motivation
----------
The default behavior of nbconvert allows one to convert a notebook to a script.
However, this is a one-way transformation. Not enough information to convert
from a script back to a notebook is exported.

This repo provides tools for converting back and forth between the two formats.
The main use case is for seamlessly transitioning between exploratory and
production-ready code, though a secondary benefit is ease of debugging
computations that have many long-running steps, and diff clarity of notebook
versions.

Usage
-----
The repository contains two scripts, `to-notebook` and `to-script`. Each of
which has one required argument, the path to the notebook or script to convert,
and one optional argument, a path to which to write the converted script or
notebook. If no target is given, the script will write a script or notebook
with the same name as the source, with the file extension changed.

```console
# Generates foo/bar.ipynb
$ to-notebook foo/bar.py

# Generates bar.ipynb
$ to-notebook foo.py bar.ipynb
```

```console
# Generates foo/bar.py
$ to-script foo/bar.ipynb

# Generates bar.py
$ to-script foo.ipynb bar.py
```

Format
------
No special markup or formatting is needed to convert notebooks to scripts. To
convert in the other direction, the script must have the same markup as is
generated by to-script.

Markdown cells are given as markdown between two lines that start and end a
multi-line string literal:

```python
'''
# Markdown Cells
Markdown cells are delimited by multiline strings. The triple-quotes must appear
as the first characters on a line (no leading whitespace), and must be on their
own line (no markdown on the same line as a triple-quote).
'''
```

Code cells are delimited with a special comment, `#>`.

```python
#>
print('This is a code cell')

#>
print('Here is another')
```

The tool has special support for notebook-only and script-only code cells.
Notebook-only code cells must start with a special comment, `#nb>`. In script
form, every line to be included in the cell must begin with a `#` character.
When converted to notebook form, this will be removed. When converting from
notebook to script form, a `#` is prepended to each line in the cell.

```python
#nb>
# print('This only executes when run in a notebook')
```

Script-only code cells behave conversely, and must start with `#py>`. The tool
will automatically add `#` to each line when converting from a script to a
notebook, and strip it out when converting in the other direction.

```python
#py>
print('This only executes when run as a script')
```

The tool inserts blank lines between each cell in script format. It is
recommended to follow this convention as well.

Examples
--------
Examples of corresponding script/notebook pairs can be found in the
test/fixtures directory.