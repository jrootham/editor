editor
======

Generalized editor system
-------------------------

The generalized editor system is designed to support editing and code generation 
(compiling) for any language that can be described with a variant of Backus Naur Format (BNFV).

In the following items in *italics* are variables.  Regular text items are literals.  Things contained in *[* *]* are optional.  

### BNFV definition

#### Comments

All source needs comments, we are using the simplest system.  Comments start with # and run to the end of the line.  

#### Structure

The general structure of a production is:
  
*production* := *[* e *]* *BNFV expression* *[* e *]* *[* ^ *compile time action* *]* *[* => *code to generate* *]* ;

The optional 'e' is a direction to the editor that the expression may be extended in that direction.

White space is in general not significant, so newlines may be inserted.

The first production is the root of the parse tree (and the starting point for the emitted code).

#### BNFV expressions 

Parentheses group items.  

An *item* preceded by a \* is repeated 0 or more times.  

*items* separated by spaces are concatenated.

*items* separated by | means one of them.  

Precedence is \* (highest), then concatenation, then | (lowest).

A reference to a production is surrounded by \< \>.  

A literal *item* is surrounded by " ".  An empty *item* is represented by "".

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

#### Builtins
  
Some expressions are built in special cases.   The
following builtins define types.

+  unsigned  *[* *bits* *]*
+  integer   *[* *bits* *]*
+  fixed *[* *bits  exponent-bits* *]*
+  float *[* *bits exponent-bits* *]*
+  fixed_bcd *[* *digits* *[* *fraction_digits* *]* *]*
+  string
+  quoted_string

*bits* and *exponent-bits* are the number of bits used to represent the number.  *digits* and *fraction-digits* 
are the number of digits in the number.  Yes, I am willing to support COBOL.

string supports the escapes "", \\t, \\n, \\r, \\\\    
There are also 

+  symbol  *scope* *[* *namespace* *]* *[* */matching regex/* *]*
+  match */matching regex/*

The matching regex defaults to /([A-Z]|[a-z])([A-Z]|[a-z]|[0-9]|_)*/

The scope is one of a small set.  This list will extend as new languages arrive.
    
+ global - all symbols go into one symbol table.
    
The namespace specifies which one of a set of symbol tables the symbol goes into.

#### Parse Time Actions

The currently available parse time actions are:
+ (insert *item*) insert the *item* into its symbol table
+ ( *item* is_inserted) trigger an compile time error if not true
+ ( *item* is_type *type*)  type is a production defining a type (see Builtins) trigger an compile time error if not true
+ ( *item1* type_equals *item2*) trigger an compile time error if not true
+ (make_type *type*) force the production to be of type *type*
+ ( *item* set *value*) set the *item* entry in the symbol table to *value*
+ (is_set *item*) trigger a compile time error if not true

#### Code Generation

() means to create a line of emitted code using the contained code.

Literal output is in " ".

Inside of () < *item*> should be an *item* in the current production that ultimately resolves to a constant or a 
symbol. The value of the constant or the name of the symbol will be inserted.

Outside of ()  < *production*> means to descend the tree along the branch represented by < *production*>.  If 
it is an concatenation connection do them from left to right.  If it is repetition do it in lexical order.


    
    


  

