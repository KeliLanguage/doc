---
description: >-
  This chapter will describe the lexical structure of Keli. Most of the
  information on this page can be ignored during the first reading of this
  report.
---

# Section 2: Lexical Structure

## 2.1 Notational Conventions

The following notations are used to described the grammar of Keli \(for more info, lookup [EBNF](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)\):

| Notation | Meaning |
| :--- | :--- |
| \[ _pattern_ \] | optional |
| { _pattern_ } | repetition |
| \( _pattern_ \) | grouping |
| _pattern1_ \| _pattern2_ | choice |
| _pattern1_ | difference; elements of _pattern1_ except those of _pattern2_ |
| `hello` | terminal syntax in typewriter font |

## 2.2 Abbreviations

The following abbreviations will be used to describe the lexical structure of Keli.

| Abbreviation | Meaning |
| :--- | :--- |
| id | identifier |
| op | operator |
| alnum | alphanumeric |

## 2.3 Lexical Program Structure

| Term | Definition |
| :--- | :--- |
| _program_ | { _lexeme_ \| _whitespace_ } |
| _lexeme_ | _id_  \| _literal_ \| _reservedOp_ |
| _id_ | _alnumId_ \| _symbolicId_ |
| _alnumId_ | \( `#` \| `?` \| `_` \| _alphabet_ \) { _alphabet_ \| _digit_ \|`#` \| `?` \| `-` \| `_`} |
| _operator_ | \( `~` \| `!` \| `@` \| `$` \| `%` \| `^` \| `&` \| `*` \| `-` \| `=` \| `+` \| `/` \| `<` \| `>` \| `:` \| `\` \) |
| _alphabet_ | `a` \| ... \|`z` \| `A` \| ... \| `Z` |
| _digit_ | `0` \| `1` \| `2` \| `3` \| `4` \| `5` \| `6` \| `7` \| `8` \| `9` |
| _literal_ | _integer_ \| _float_ \| _string_ |
| _integer_ | _decimal_ |
| _float_ | _decimal_ `.` _decimal_ |
| _decimal_ | _digit_ { _digit_ } |
| _string_ | `"` { _space_ \| _escape_ \| _gap_ } `"` |
| _reservedOp_ | `=` |
| _whitespace_ | _whitestuff_ { _whitestuff_ } |
| _whitestuff_ | _whitechar_ \| _comment_ |
| _whitechar_ | _newline_ \| _vertab_ \| _space_ \| _tab_ \| _unicodeWhitespace_ |
| _newline_ | _return linefeed_ \| _return_ \| _linefeed_ \| _formfeed_ |
| _return_ | a carriage return |
| _linefeed_ | a line feed |
| _vertab_ | a vertical tab |
| _formfeed_ | a form feed |
| _space_ | a space |
| _tab_ | horizontal tab |
| _unicodeWhitespace_ | any Unicode character defined as whitespace |
| _comment_ | _slashes_ \[ _nonnewline_ { _nonnewline_ }\] _newline_ |
| _slashes_ | `/` `/` { `/` } |

## 2.4 Comments

Comments are actually whitespaces. Comments starts with 2 slahses and extends until before the following newline. For example:

```java
// this is a comment
```

## 2.5 Constant Identifiers

A constant identifier is any sequences of characters that satisfy the following grammar:

\( `$` \| `_` \| _alphabet_ \) { _alphabet_ \| _digit_ \| `-` \| `_`}

For example:

| Term | Validity |
| :--- | :--- |
| `hello` | Valid |
| `_` | Valid |
| `true` | Valid |
| `snake_case` | Valid |
| `aVariable` | Valid |
| `x123` | Valid |
| `crazy_stuff_lol` | Valid |
| `Content-Type` | Valid |
| `12x` | Invalid |

Constant identifiers are used for identifying:

1. Function parameter 
2. Type 
3. Object property 
4. Type variable 

## 2.6 Function identifiers

Function identifiers are used for identifying functions, they can be any sequences of characters that satisfy the following grammar:

> [_constId_](chapter-2-lexical-structure.md#2-5-constant-identifiers) \| \({_operator_ }\) &lt;_reservedOp_&gt;

In layman term, a function identifier can be a constant identifiers OR a sequences of symbols that are not reserved operator.

Examples of function identifiers:

| Term | Validity |
| :--- | :--- |
| `plus` | Valid |
| `toString` | Valid |
| `+` | Valid |
| `->` | Valid |
| `==` | Valid |
| `=` | Invalid, reserved operator |

