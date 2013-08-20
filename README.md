editor
======

Generalized editor system
-------------------------

The generalized editor system is designed to support editing and code generation 
(compiling) for any language that can be described with a variant of Backus Naur Format (BNFV).

In the following items in *italics* are variables.  Regular text items are literals.  Things contained in *[* *]* are optional.  

### BNFV definition

#### Comments

All source needs comments, we are using the simplest system.  All lines that start with # are comments.

#### Structure

The general structure of a production is:
  
*production* := *BNFV expression* *[* ^ *compile time action* *]* *[* => *code to generate* *]* ;

White space is in general not significant, so newlines may be inserted.

The first production is the root of the parse tree (and the starting point for the emitted code).

#### BNFV expressions 

Parentheses group items.  

An *item* followed by a \* is repeated 0 or more times.  

*items* separated by spaces are concatenated.

*items* separated by | means one of them.  

A reference to a production is surrounded by \< \>.  

A literal *item* is surrounded by " ".  An empty item is represented by "".

Syntacticly marginal whitespace is represented by [ *[* *contents* *]* ] or { *[* *contents* *]*}.  [ ] requires 
at least 1 whitespace character to parse correctly.  { } does not.  The *contents* describe the whitespace 
that is to be inserted on output.  

The options are:

+ s a space
+ t a tab
+ n a newline
+ i indent to current level
+ + increase the current level and indent
+ - decrease the current level and indent
+ c *n* skip to column *n* unless we are already past it, in which case insert a tab (there is no space after the c, 
  it is a markdown artifact).
+ w if wrapping is required preferentially wrap here

These can be combined, the most likely combinations are ni, n+ and n-.

#### Compile Time Actions

The currently available compile time actions are:
+ \< insert *item*\> insert the *item* into its symbol table
+ \< *item* isInserted\> trigger an compile time error if not true
+ \< *item* isType *type*\>  type is a production defining a type (see Builtins) trigger an compile time error if not true
+ \< *item1* typeEquals *item2*\> trigger an compile time error if not true
+ \< makeType *type*\> force the production to be of type *type*
+ \< *item* set *value*\> set the *item* entry in the symbol table to *value*
+ \< isSet *item*\> trigger a compile time error if not true

#### Code Generation

Literal output is in " ".

< *item*> takes the value of the code generated by the *item* in the current production.

#### Builtins
  
Some expressions are built in special cases.  These may appear only by themselves on the right side of :=.  The
following builtins define types.

+  unsigned  *[* *bits* *]*
+  integer   *[* *bits* *]*
+  fixed *[* *bits  exponent-bits* *]*
+  float *[* *bits exponent-bits* *]*
+  fixed-bcd *[* *digits* *[* *fraction-digits* *]* *]*
+  string

*bits* and *exponent-bits* are the number of bits used to represent the number.  *digits* and *fraction-digits* 
are the number of digits in the number.  Yes, I am willing to support COBOL.

string supports the escapes "", \\t, \\n, \\r, \\\\    
There is also 

+  symbol  *[* */matching regex/* *]* *scope* *[* *namespace* *]*

The matching regex defaults to /([A-Z]|[a-z])([A-Z]|[a-z]|[0-9]|-|_)*/

The scope is one of a small set.  This list will extend as new languages arrive.
    
+ global - all symbols go into one symbol table.
    
The namespace specifies which one of a set of symbol tables the symbol goes into.
    
    


  

