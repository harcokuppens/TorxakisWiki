<a name="Enable-Operator"></a>

# Enable Operator[¶](#Enable-Operator)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

[processBehaviour](ProcessBehaviour) >>> [processBehaviour](ProcessBehaviour)  
[processBehaviour](ProcessBehaviour) >>> "ACCEPT" ("?" [varDecl](VarDecl) | "!" [valExpr](ValExpr) )* IN [processBehaviour](ProcessBehaviour) NI

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

Synchronize on EXIT of multiple processes.  
When EXIT values are communicated, they must be ACCEPTed for further use.

<a name="Examples"></a>

## Examples[¶](#Examples)

<pre>(
    A ? x :: Int >-> EXIT ! x ? y :: Int
|||
    B ? y :: Int >-> EXIT ? x :: Int ! y
) 
>>> ACCEPT ? a ? b IN 
   C ! a + b
NI
</pre>