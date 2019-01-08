<a name="Sequence-Operator"></a>

# Sequence Operator[¶](#Sequence-Operator)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

[communications](Communications) ">->" [processBehaviour](ProcessBehaviour)

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

After the communications have occurred (in a single step), the described process behaviour is exposed.

<a name="Examples"></a>

## Examples[¶](#Examples)

The statement  

<pre>Channel1_Int ? x >-> Channel2_Int ! x
</pre>

describes the process that

*   first communicates variable `x` over `Channel1_Int`
*   and next communicates the same variable on `Channel2_Int`

The statement  

<pre>Channel1_Int ? x | Channel2_Int ? y >-> P[Channel3_Int](x,y)
</pre>

describes the process that

*   first simultaneously communicates variable `x` over `Channel1_Int` and variable `y` over `Channel2_Int`
*   and next instantiates the process `P` with `Channel3_Int` as channel and the value of the variables `x` and `y` as arguments.
