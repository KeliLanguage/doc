---
description: This page shall describe the syntax of Keli.
---

# Syntax

As mentioned [previously](./), the syntax of Keli should be pure, let's check it out.

## **Preface**

**There are ZERO keywords in Keli**. There are only predefined constants \(or singletons\). 

## Grammar

The grammar of a Keli is rather simple, as follows \(described in [EBNF](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form)\):

Notes: 

* `[]` means optional
* `{}` means repetition

```text
keli_program 
    = {decl}

decl 
    = const_decl 
    | func_decl
    
const_decl 
    = [id ':' [type]] '=' expr

func_decl 
    = [generics] param ',' (fid | {fid param}) '|' ret_type '=' expr

generics
    = '{' {param} '}'

param
    = id ':' type

type 
    = atomic_expr

ret_type
    = expr

expr
    = atomic_expr
    | func_call
    | lambda

atomic_expr
    = '(' expr ')'
    | number_lit
    | string_lit
    
func_call
    = expr ',' (fid | {fid atomic_expr})

lambda
    = {id} '|' expr 
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
| `,` | Comma | For invoking and chaining functions. |
| `:` | Colon | For annotating types. |
| `()` | Parentheses | For grouping expressions |
| `[]` | Square brackets | For declaring a list expression |
| `{}` | Curly braces | For defining generic params |
| `"` | Double quote | For enclosing string literal |
| `_` | Underscore | A symbol that can be used in identifiers. |
| `?` | Question mark | A symbol that can be used in identifiers. |



