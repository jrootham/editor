editor
======

Generalized editor system
-------------------------

The generalized editor system is designed to support editing and code generation 
(compiling) for any language that can be described with a variant of Backus Naur Format (BNFV).

The BNFV looks like this:

Items in *italics* are variables.  Regular text items are literals.  Parentheses group items.  An item followed 
by a \* is repeated 0 or more times.  Items contained in [ ] are optional.  Items separated by spaces are concatenated, 
items separated by | means one of them.  A reference to a previously defined production is surrounded by \< \>.  An 
empty item is represented by "".



1.  Comments

  All source needs comments, we are using the simplest system.  All lines that start with # are comments.

2.  Structure
 
  The general structure of a line is:
  
  *production* := *BNFV expression* [ => \< *code to generate* \> ]

  The first line is the root of the parse tree (and the starting point for the emitteed code).
  
3.  Builtins
  
  Some expressions are built in special cases, these may have parameters, possibly optional.  These may appear only 
by themselves on the right side of :=.

    1.  unsigned  [ *bits* ]
  
    2.  integer   [ *bits]* 
  
    3.  fixed [ *bits  exponent-bits* ]
    
    5.  float [ *bits exponent-bits* ]

    6.  fixed-bcd [ *digits* [ *fraction-digits* ]]
  
        Yes, I am willing to support COBOL
        
    7.  symbol  [ */matching regex/* ] *scope* [ *namespace* ]
    
    The matching regex defaults to /([A-Z]|[a-z])([A-Z]|[a-z]|[0-9]|-|_)*/

    The scope is one of a small set.  This list will extend as new languages arrive.
    
      1. global - all symbols go into one symbol table.
    
    
    The namespace specifies which one of a set of symbol tables the symbol goes into.
    
    


  

