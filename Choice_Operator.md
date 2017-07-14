<a name="Choice-Operator"></a>

# Choice Operator[¶](#Choice-Operator)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

[processBehaviour](ProcessBehaviour) "##" [processBehaviour](ProcessBehaviour)

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

process1 ## process2

Either process1 or process2 is executed, but never both.

<a name="Examples"></a>

## Examples[¶](#Examples)

The statement  

<pre>    Channel1_Int ? x 
##
    Channel2_Int ? y
</pre>

describes the process that  
either communicates the variable x over Channel1_Int  
or communicates the variable y over Channel2_Int.