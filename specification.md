h1. Language Definition

h2. Syntax

|specification
|( [[modelDefs]]
 &#124; [[cnectDefs]]
 &#124; [[chanDefs]]
 &#124; [[typeDefs]]
 &#124; [[constDefs]]
 &#124; [[funcDefs]]
 &#124; [[procDefs]]
 &#124; [[stautDef]]
) *
|

h2. Semantics

A specification contains zero or more definitions.
A [[modelDefs|Model Definition]] is needed to step a TorXakis model.
A [[modelDefs|Model Definition]] and [[cnectDefs|Connection Definition]] are minimally needed to test a SUT or simulate a system using a TorXakis model.