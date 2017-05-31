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
A [[Model Definition|modelDefs]] is needed to step a TorXakis model.
A [[Model Definition|modelDefs]] and [[Connection Definition|cnectDefs]] are minimally needed to test a SUT or simulate a system using a TorXakis model.