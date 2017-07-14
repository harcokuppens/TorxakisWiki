<a name="Data-Type"></a>

# Data Type[¶](#Data-Type)

TorXakis has predefined and user defined data types.

<a name="Predefined-Data-Types"></a>

## Predefined Data Types[¶](#Predefined-Data-Types)

TorXakis has the following four predefined data types.

<table>

<tbody>

<tr>

<th>data type name  
</th>

<th>description  
</th>

</tr>

<tr>

<td>Bool  
</td>

<td>Boolean value: True and False  
</td>

</tr>

<tr>

<td>Int  
</td>

<td>Unbounded Integer values  
</td>

</tr>

<tr>

<td>String  
</td>

<td>“Sequence of ASCII characters”  
</td>

</tr>

<tr>

<td>Regex  
</td>

<td>[‘W3C XSD Regular Expression String’](http://www.w3.org/TR/xmlschema11-2/#regexs)  
Note: The regular expressions are also translate to [POSIX](https://en.wikibooks.org/wiki/Regular_Expressions/POSIX-Extended_Regular_Expressions)  
Some limitations of POSIX are not solved.  
Consequence: regular expression like (a{0}) and [a-\]] will fail. Of course, they can be rewritten to a POSIX valid regular expression.  
</td>

</tr>

</tbody>

</table>

<a name="User-Defined-Data-Types"></a>

## User Defined Data Types[¶](#User-Defined-Data-Types)

In TorXakis, one can defined datatypes, including unions, Cartesian Products, and recursive types, using  
[TYPEDEF](TypeDefs).