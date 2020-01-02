# Tokenized Bytecode
This document is the entire lexical token bytecode specification. This format applies to and only to lexical tokens. Other passes may share specifications with this bytecode but are in no way obligated to follow the same format.

## Table of Contents
* Available Tokens
    - Keyword Tokens
        + Def Token
        + Let Token
    - Delimiting Tokens
    - Other Tokens
        + Identifier Token
        + IntegerLiteral Token
        + End of File Token
* Overall Bytecode Specifications
    - Head Bytecode
        + Token Start
        + Token ID
    - Body Bytecode
        + Token Data
            * Token Data Start
            * Token Data
            * Token Data End
        + Token Location
            * Token Location Start
            * Token Location File Path
            * Token Location Line Number
            * Token Location Column Number
            * Token Location End
    - Tail Bytecode
        + Token End

### Available Tokens
All token ID's are 16-bit (2 byte integers) where the high byte represents the token category id and the low byte is the specific token id.

#### Keyword Tokens
A keyword token is a token that represents any keyword. Beagle only has two hard keywords currently and all other keywords are up for interpretation by the parser (aka soft keywords).

A keyword token high byte is always `0xa1`

```
Def: 0x10
Let: 0x20 
```

#### Delimiting Tokens
A delimiting token is a token that represents some kind of delimiting character like '(', ':', ''', etc. These tokens are only single character tokens and any intepretation of poly-character tokens are left for the parser to decide (such as '==', or '?:', etc).

A delimiting token high byte is always `0xa2`

```
= -> 0xa1
: -> 0xa2
( -> 0xa3
) -> 0xa4
{ -> 0xa5
} -> 0xa6
+ -> 0xa7
- -> 0xa8
* -> 0xa9
/ -> 0xaa
\ -> 0xab
" -> 0xac
' -> 0xad
! -> 0xae
_ -> 0xaf
. -> 0xb0
, -> 0xb1
; -> 0xb2
& -> 0xb3
% -> 0xb4
# -> 0xb5
@ -> 0xb6
| -> 0xb7
? -> 0xb8
> -> 0xb9
< -> 0xba
^ -> 0xbb
[ -> 0xbc
] -> 0xbd
```

#### Other Tokens
Other tokens include non-delimiting tokens and non-keyword tokens, such as identifiers, literals, ISO controls such as CR, LF, EOF, etc.

An other token high byte is always `0xa3`

```
Identifier: 0x3a
Integer Literal: 0x3b
End of File: 0x3c 
```
Some tokens will be representing arbitrary data such as an identifier or an integer literal. This can be shown in bytes in the following format:

```
Data Start: 0xb1
Data Body: ???
Data End: 0xb2
```

### Token Location Data
A token location is a data object that represents the location of a token. This includes the file path (in the future, this will be split into different kinds of input such as file input, console input, etc), the line number, and the column number (in the future there will be a column start and column end). This will be shown as bytes in the following format:

```
Token Location Start: 0xc0
Token Location File Path Start: 0xc1
Token Location File Path Data: ???
Token Location File Path End: 0xc2
Token Location Line Number Start: 0xc3
Token Location Line Number Data: ???
Token Location Line Number End: 0xc4
Token Location Column Number Start: 0xc5
Token Location Column Number Data: ???
Token Location Column Number End: 0xc6
Token Location End: 0xcf
```

### Overall Bytecode Specification
The overall bytecode specification is as follows:

```
|------------------------------------------------|
|                 *Token Header*                 |
|------------------------------------------------|
|      Token Start (0xfe)    |     Token Id      |
|------------------------------------------------|
|                  *Token Body*                  |
|------------------------------------------------|
| Token Data Start | Token Data | Token Data End |
|------------------------------------------------|
|                  Token Location                |
|------------------------------------------------|
| Token Location Start | File Path | Line Number |
|------------------------------------------------|
|    Column Number  |    Token Location End      |
|------------------------------------------------|
|                   *Token Tail*                 |
|------------------------------------------------|
|                 Token End (0xff)               |
|------------------------------------------------|
```

### Examples
Def Token in file test.bg at line 1, column 1:
```
0xfe 
    0xa1 0x10 
    0xc0 
        0xc1 
            0x74 0x65 0x73 0x74 0x2e 0x62 0x67 
        0xc2 
        0xc3 
            0x00 0x00 0x00 0x01 
        0xc4 
        0xc5 
            0x00 0x00 0x00 0x01 
        0xc6 
    0xcf
0xff
```