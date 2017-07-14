<a name="Value-Expression"></a>

# Value Expression[¶](#Value-Expression)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>valExpr  
</td>

<td>  [letValExpr](LetValExpr)  
| [iteValExpr](IteValExpr)  
| [valExpr](ValExpr)? operator [valExpr](ValExpr)  
| [valExpr](ValExpr) "::" typeName  
| constName  
| varName  
| funcName "(" neValExprList? ")"  
| constructorName ("(" neValExprList ")")?  
| Integer  
| String  
| "REGEX" "(" RegexVal ")"  
| "(" [valExpr](ValExpr) ")"  
| "ERROR" String  
</td>

</tr>

<tr>

<td>operator  
</td>

<td>todo  
</td>

</tr>

<tr>

<td>neValExprList  
</td>

<td>[valExpr](ValExpr) ("," [valExpr](ValExpr))*  
</td>

</tr>

<tr>

<td>typeName  
</td>

<td>[CapsId](CapsId)  
</td>

</tr>

<tr>

<td>constName  
</td>

<td>[SmallId](SmallId)  
</td>

</tr>

<tr>

<td>varName  
</td>

<td>[SmallId](SmallId)  
</td>

</tr>

<tr>

<td>funcName  
</td>

<td>[SmallId](SmallId)  
</td>

</tr>

</tbody>

</table>

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

<a name="Reference-to-Function"></a>

### Reference to Function.[¶](#Reference-to-Function)

The reference points to either a predefined function name,  
a function name implicitly defined by a type definition,  
or the function name of a user defined function.

Include types?