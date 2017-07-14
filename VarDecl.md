<a name="Variable-Declaration"></a>

# Variable Declaration[¶](#Variable-Declaration)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>varDecl  
</td>

<td>varName ("::" typeName)?  
</td>

</tr>

<tr>

<td>varName  
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

Declare a variable. The type can only be left out when the type can be deducted from the context.

<a name="Examples"></a>

## Examples[¶](#Examples)

The statement  

<pre>s :: String
</pre>

declares a variable named s of type String.