# Language Definition

## Syntax

specification = 
( [[modelDefs]]
 &#124; [[cnectDefs]]
 &#124; [[chanDefs]]
 &#124; [[typeDefs]]
 &#124; [[constDefs]]
 &#124; [[funcDefs]]
 &#124; [[procDefs]]
 &#124; [[stautDefs]]
) *


## Semantics

A specification contains zero or more definitions.
* To step a specification at least a [[Model Definition|modelDefs]] and [[Channel Definition|chanDefs]] are needed.
* To test a SUT at least a [[Model Definition|modelDefs]], [[Channel Definition|chanDefs]] and [[Connection Definition|cnectDefs]] are needed.
* To simulate a system at least a [[Model Definition|modelDefs]], [[Channel Definition|chanDefs]] and [[Connection Definition|cnectDefs]] are needed.