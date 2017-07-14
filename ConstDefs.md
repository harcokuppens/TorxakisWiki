<a name="Constant-Definitions"></a>

# Constant Definitions[¶](#Constant-Definitions)

In TorXakis, the user can define constants using the CONSTDEF keyword.

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>constDefs  
</td>

<td>"CONSTDEF" constDef (";" constDef)* "ENDDEF"  
</td>

</tr>

<tr>

<td>constDef  
</td>

<td>constName "::" typeName "::=" [valExpr](ValExpr)  
</td>

</tr>

<tr>

<td>constName  
</td>

<td>[SmallId](SmallId)  
</td>

</tr>

<tr>

<td>typeName  
</td>

<td>[CapsId](CapsId)  
</td>

</tr>

</tbody>

</table>

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

Define constants.  
The ValExpr must yield a constant value of the same [data type](Data_Type) as referenced to by the typeName.

<a name="Examples"></a>

## Examples[¶](#Examples)

The following statement declares two integer constants, named max and min, with the values 255 and 0, respectively.  

<pre>CONSTDEF
   max :: Int ::= 255 ;
   min :: Int ::= 0
ENDDEF
</pre>