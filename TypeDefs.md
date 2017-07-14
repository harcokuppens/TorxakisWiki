<a name="Type-Definitions"></a>

# Type Definitions[¶](#Type-Definitions)

In TorXakis, the user can define data types using the TYPEDEF keyword.

TorXakis will generate equality, type checking, and accessors [functions](Function) for the user defined data types.

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>typeDefs  
</td>

<td>"TYPEDEF" typeDef (";" typeDef)* "ENDDEF"  
</td>

</tr>

<tr>

<td>typeDef  
</td>

<td>typeName "::=" neConstructorList  
</td>

</tr>

<tr>

<td>neConstructorList  
</td>

<td>constructor ("|" constructor)*  
</td>

</tr>

<tr>

<td>constructor  
</td>

<td>constructorName ("{" neFieldsList "}")?  
</td>

</tr>

<tr>

<td>neFieldsList  
</td>

<td>fields (";" fields)*  
</td>

</tr>

<tr>

<td>fields  
</td>

<td>neFieldNameList "::" typeName  
</td>

</tr>

<tr>

<td>neFieldNameList  
</td>

<td>fieldName ("," fieldName )*  
</td>

</tr>

<tr>

<td>typeName  
</td>

<td>[CapsId](CapsId)  
</td>

</tr>

<tr>

<td>constructorName  
</td>

<td>[CapsId](CapsId)  
</td>

</tr>

<tr>

<td>fieldname  
</td>

<td>[SmallId](SmallId)  
</td>

</tr>

</tbody>

</table>

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

Define data types.  
A data type has a unique typeName.

A data type must contain one or more constructors.  
A data type cannot contain constructors with the same constructorName.

A constructor can contain zero or more fields.  
A data type cannot contain fields with the same fieldname.  
Hence, a constructor cannot contain fields with the same fieldname,  
and multiple constructors cannot contain a field with the same fieldname.

The typeName associated with fields refers to a [data type](Data_Type): either a [predefined data type](Data_Type) or a [user defined data type](TypeDefs).  
A data type is called recursive when one or more fields refer to itself.

<a name="Examples"></a>

## Examples[¶](#Examples)

The statement  

<pre>TYPEDEF Point ::= CPoint { x, y :: Int } ENDDEF
</pre>

defines the data type Point as the Cartesian product of two integers. These integers are referred to as x and y.

The statement  

<pre>TYPEDEF Conditional_Int ::=
          CAbsent_Int
        | CPresent_Int { value :: Int }
ENDDEF
</pre>

defines the data type Conditional_Int as the union of the absence of a value and the presence of any integer value.

The statement  

<pre>TYPEDEF List_Int ::=
         CNil_Int    
      |  Cstr_Int { head :: Int; tail :: List_Int } ENDDEF
</pre>

defines the recursive data type List_Int as the union of the empty list and the list constructor: The Cartesian product of an integer referred to as head, and a List_Int referred to as tail.