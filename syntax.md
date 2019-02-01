# Syntax

As mentioned [previously](./), the syntax of Keli should be pure, let's check it out.

## **Preface**

**There are ZERO keywords in Keli**. There are only predefined constants \(or singletons\). 

## Grammar

The grammar of a Keli is rather simple, as follows \(described in [EBNF](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form)\):

Notes: 

* `[]` means optional
* `{}` means repetition

```javascript
keliProgram 
    = {decl}

decl 
    = constDecl 
    | funcDecl
    
constDecl 
    = [id] '=' expr

funcDecl 
    = [generics] param '.' (fid | {fid param}) ['|' returnType] '=' expr

generics
    = '{' param '}'

param
    = '(' id type ')'

type 
    = expr

returnType
    = expr

expr
    = funcCall
    | lambda 
    | numberLit
    | stringLit
    | '(' expr ')'
    
funcCall
    = expr '.' (fid | {fid '(' expr ')'})

lambda
    = id '|' expr 
```

## Glossary

| Abbreviation | Meaning |
| :--- | :--- |
| decl | declaration |
| func | function |
| expr | expressions |
| lit | literal |
| id | identifier |
| const | constant |
| mono | one  |
| poly | more than one  |
| fid | function identifier |
| param | parameter |
| ret | return |



## Reserved operator

The following is a list of reserved operators in Keli, which means you cannot use them as function identifiers.

| Symbol | Name | Purpose |
| :--- | :--- | :--- |
| `=` | Equal | Used in declarations |
| `|` | Pipe | As the placeholder for function return types and lambdas. |
| `.` | Period | For invoking and chaining functions. |
| `()` | Parentheses | For grouping expressions |
| `[]` | Square brackets | For declaring a list expression |
| `{}` | Curly braces | For defining generic params |
| `"` | Double quote | For enclosing string literal |
| `_` | Underscore | A symbol that can be used in identifiers. |
| `?` | Question mark | A symbol that can be used in identifiers. |

## Identifiers

Any identifiers in Keli must obey the following rules:

* The first character must be an lowercase/uppercase alphabet or an underscore symbol.
* The following characters can be any alphanumeric character including the underscore symbol and the question mark symbol.

Examples as follows:

| Identifier | Validity |
| :--- | :--- |
| `hello` | Valid |
| `_` | Valid |
| `myCar` | Valid |
| `even?` | Valid |
| `person1` | Valid |
| `snake_case` | Valid |
| `123hi` | Invalid, first letter can't be digit. |

