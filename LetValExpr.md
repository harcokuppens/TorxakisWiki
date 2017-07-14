<a name="LET-Value-Expression"></a>

# LET Value Expression[¶](#LET-Value-Expression)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>letValExpr  
</td>

<td>"LET" assignment (";" assignment)* "IN" [valExpr](ValExpr) "NI"  
</td>

</tr>

<tr>

<td>assignment  
</td>

<td>[varDecl](VarDecl) "=" [valExpr](ValExpr)  
</td>

</tr>

</tbody>

</table>

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

Introduce variables

<a name="Examples"></a>

## Examples[¶](#Examples)

<a name="Simultaneously-define-multiple-variables"></a>

### Simultaneously define multiple variables[¶](#Simultaneously-define-multiple-variables)

The statement  

<pre>LET a = 5; b = 3; c = 8 IN
   ...
   LET a = 1+b; b = 2+a; c = a*b IN ... NI
   ...
NI
</pre>

defines two times three new variables (a, b, and c).  
Inside the first LET IN NI block except the second LET IN NI block, the values of a, b, and c are 5, 3, and 8, respectively.  
Inside the second LET IN NI block, the values of a, b, and c are 4, 7, and 15, respectively.