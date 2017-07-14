<a name="Guard-Operator"></a>

# Guard Operator[¶](#Guard-Operator)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>guardedProcessBehaviour  
</td>

<td>[condition](Condition) =>> [processBehaviour](ProcessBehaviour)  
</td>

</tr>

<tr>

<td>[condition](Condition)  
</td>

<td>[[ [valExpr](ValExpr) ]]  
</td>

</tr>

</tbody>

</table>

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

Conditional execution: [[ expr ]] =>> next  
Next is only executed when expr is true

<a name="Example"></a>

## Example[¶](#Example)

[[ not(isNil(buf)) ]] =>> Value !head(buf)