## ex3


### Escape codes

These are pretty much the same as Python's. http://en.wikipedia.org/wiki/Escape_sequences_in_C 

The most useful of these are (at least I have parsed docs with them):
- `\n` : Newline
- `\r` : Carriage Return
- `\t` : Horizontal Tab
- `\\ \' \" \?` : slash, (double)quote, and questionmark

Ones i've never used before:
- `\a` : Alarm..beep/bell.. super useful for villains.
- `\b` : backspace, maybe that's what makes progress indicators work?
- `\f` : formfeed, not sure.

Didn't know about these, but seem useful
- `\v` : Vertical Tab.. wonder how to abuse that.
- `\nnn` : nnn interpreted as an octal number
- `\xhh` : hh interpretted as hex

### String format sequences

more at : http://www.codingunit.com/printf-format-specifiers-format-conversions-and-formatted-output

- `%f` : float
- `%i o %d` : int
- `%c` : char
- `%s` : string

- `%3d` : packs to fill 3 positions `4` -> `` 4`
- `%03d` : packs to fill 3 positions `4` -> `004`
- `%3.2f` : TODO.. or wait till you really need this .