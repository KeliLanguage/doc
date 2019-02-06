---
description: >-
  This chapter will describe the lexical structure of Keli. Most of the
  information on this page can be ignored during the first reading of this
  report.
---

# Chapter 2: Lexical Structure

## 2.1 Notational Conventions

The following notations are used to described the grammar of Keli \(for more info, lookup [EBNF](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)\):

| Notation | Meaning |
| :--- | :--- |
| \[ _pattern_ \]  | optional |
| { _pattern_ } | repetition |
| \( _pattern_ \) | grouping |
| _pattern1_ \| _pattern2_ | choice |
| _pattern1_&lt;_pattern2_&gt; | difference - elements of _pattern1_ except those of _pattern2_ |
| `hello` | terminal syntax in typewriter font |

At the moment, Keli program uses the ASCII character set, Unicode character set shall be incorporated in the future once a stable version of Keli is released.

## 2.2 Abbreviations

The following abbreviations will be used to describe the lexical and grammar structure of Keli.

| Abbreviation | Meaning |
| :--- | :--- |
| decl | declaration |
| func | function |
| expr | expressions |
| lit | literal |
| id | identifier |
| const | constant |
| mono | one |
| poly | more than one |
| fid | function identifier |
| param | parameter |
| ret | return |
| op | operator |
| alnum | alphanumeric |

## 2.3 Lexical Program Structure

| Term | Definition |
| :--- | :--- |
| _program_ |  { _lexeme_ \| _whitespace_ } |
| _lexeme_ | _id_  \| _literal_ \| _reservedOp_  |
| _id_ | _alnumId_ \| _symbolicId_ |
| _alnumId_ | \( `#` \| `?` \| `_` \| _alphabet_ \) { _alphabet_ \| _digit_ \|`#` \| `?` \| `_`} |
| _symbolicId_ |  |
| _alphabet_ |  |
| _digit_ |  |
| _literal_ | _integer_ \| _float_ \| _string_ |
| _integer_ |  |
| _float_ |  |
| _string_ |  |
| _reservedOp_ | `=` __\| `|` \| `{` \| `}` \| `(` \| `)` \| `.`  |
| _whitespace_ | _whitestuff_ { _whitestuff_ } |
| _whitestuff_ | _whitechar_ \| _comment_  |
| _whitechar_ | _newline_ \| _vertab_ \| _space_ \| _tab_ \| _unicodeWhitespace_ |
| _newline_ | _return linefeed_ \| _return_ \| _linefeed_ \| _formfeed_  |
| _return_ | a carriage return |
| _linefeed_ | a line feed |
| _vertab_ | a vertical tab |
| _formfeed_ | a form feed |
| _space_ | a space  |
| _tab_ | horizontal tab |
| _unicodeWhitespace_ | any Unicode character defined as whitespace |
| _comment_ | _slashes_ \[ _nonnewline_ { _nonnewline_ }\] _newline_ |
| _slashes_ | `/` `/` { `/` } |
| \_\_ |  |



