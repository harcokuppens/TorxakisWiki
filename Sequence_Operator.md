<a name="Sequence-Operator"></a>

# Sequence Operator[¶](#Sequence-Operator)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

[communications](Communications) ">->" [processBehaviour](ProcessBehaviour)

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

After the process has communicated, the described process behaviour is exposed.

<a name="Examples"></a>

## Examples[¶](#Examples)

The statement  

<pre>Channel1_Int ? x >-> Channel2_Int ! x
</pre>

describes the process that

*   first communicates variable x over Channel1_Int
*   and next communicates the same variable on Channel2_Int