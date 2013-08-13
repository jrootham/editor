editor
======

Generalized editor system
-------------------------

The generalized editor system is designed to support editing and code generation 
(compiling) for any language that can be described with a variant of Backus Naur Format (BNFV).

The BNFV looks like this:

1.  Comments

  All source needs comments, we are using the simplest system.  All lines that start with # are comments.

2.  Structure
 
  The general structure of a line is:
  
  production := \<BNFV expression\> => \<code to generate\>

3.  Builtins
  
  Some expressions are built in, these may have parameters, possibly [optional].  

    1.  unsigned  [bits]
  
    2.  integer   [bits]
  
    3.  fixed [bits  exponent-bits]
    
    5.  float [bits exponent-bits]

    6.  fixed-bcd [digits [fraction-digits]]
  
    7.  symbol  [/matching regex/] scope [namespace]
    
    The matching regex defaults to /([A-Z]|[a-z])([A-Z]|[a-z]|[0-9]|-|_)*/

    The scope is one of a small set.  This list will extend as new languages arrive.
    
      1. global - all symbols go into one symbol table.
    
    
    The namespace specifies which one of a set of symbol tables the symbol goes into.
    
    


  

