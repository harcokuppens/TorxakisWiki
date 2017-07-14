<a name="Function-Defintions"></a>

# Function Defintions[¶](#Function-Defintions)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>funcDefs  
</td>

<td>"FUNCDEF" funcDef (";" funcDef)* "ENDDEF"  
</td>

</tr>

<tr>

<td>funcDef  
</td>

<td>funcName "(" neVarsDeclarationList? ")" "::" typeName "::=" [valExpr](ValExpr)  
</td>

</tr>

<tr>

<td>neVarsDeclarationList  
</td>

<td>varsDeclaration (";" varsDeclaration )*  
</td>

</tr>

<tr>

<td>varsDeclaration  
</td>

<td>neVarNameList "::" typeName  
</td>

</tr>

<tr>

<td>neVarNameList  
</td>

<td>varName ("," varName )*  
</td>

</tr>

<tr>

<td>funcName  
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

<td>typeName  
</td>

<td>[CapsId](CapsId)  
</td>

</tr>

</tbody>

</table>

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

Define functions.

<a name="Examples"></a>

## Examples[¶](#Examples)

Function to determine whether a integer is an int32; a 32-bits integer.  

<pre>FUNCDEF isValid_int32 ( x :: Int ) :: Bool ::= (-2147483648 <= x) /\ (x <= 2147483647) ENDDEF
</pre>

Function to determine the validity of a password. In this example, a valid password is a string whose length is larger than 8, and that contains a capital, a small letter, and a digit.  

<pre>FUNCDEF validPassword ( pw :: String ) :: Bool ::=
      len(pw) > 8
   /\ strinre(pw, REGEX('.*[A-Z].*'))
   /\ strinre(pw, REGEX('.*[a-z].*'))
   /\ strinre(pw, REGEX('.*[0-9].*'))
ENDDEF
</pre>

Recursive function to determine the length of an instance of the recursive data type List_Int:  

<pre>FUNCDEF lengthList_Int ( x :: List_Int ) :: Int ::=
   IF isCNil_Int(x) THEN
      0
   ELSE
      1 + lengthList_Int(tail(x))
   FI
ENDDEF
</pre>

Function with multiple variables that checks for equality.  

<pre>FUNCDEF allEqual ( xi, yi :: Int; xs, ys :: String; xl, yl :: List_Int ) :: Bool ::=
   (xi == yi) /\ (xs == ys) /\ (xl == yl)
ENDDEF
</pre>

Definition of multiple functions in a single FUNCDEF block.  

<pre>FUNCDEF 
   max ( x, y :: Int ) :: Int ::= IF (x > y) THEN x ELSE y FI ;
   min ( x, y :: Int ) :: Int ::= IF (x < y) THEN x ELSE y FI
ENDDEF
</pre>