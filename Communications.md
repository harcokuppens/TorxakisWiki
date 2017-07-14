<a name="Communications"></a>

# Communications[¶](#Communications)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>communications  
</td>

<td>communication ("[|](Synchronous_Operator)" communication)* ([condition](Condition))?  
</td>

</tr>

<tr>

<td>communication  
</td>

<td>channelName ("!" [valExpr](ValExpr) | "?" [varDecl](VarDecl) )*  
</td>

</tr>

<tr>

<td>channelName  
</td>

<td>[CapsId](CapsId)  
</td>

</tr>

</tbody>

</table>

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

Specify communication.  
The direction of communication is NOT specified.

Zero or more data can be communicated.  
The list of data types of the provided data must match  
the list of data types as specified in the channel definition.

The operator ! denotes that the data item is fully specified: a known value is communicated.  
The operator ? denotes that the data item is underspecified: (part of) the value that is communicated is unknown.

The predefined channel [EXIT](EXIT) must be used to communicate process termination.  
Communication over the predefined channel [EXIT](EXIT) might include exit values.

<a name="Examples"></a>

## Examples[¶](#Examples)

<a name="Communication-without-data"></a>

### Communication without data[¶](#Communication-without-data)

Let Channel be a channel over which can be communicated without any data.  
The statement  

<pre>Channel
</pre>

specifies that the process wants to communicate.

Besides channels that communicate without any data,  
this construct can also be used to obtain simpler models by abstracting away the actual data that is communicated.

<a name="Communication-of-fully-specified-data"></a>

### Communication of fully specified data[¶](#Communication-of-fully-specified-data)

Let Channel_Int be a channel over which one integer at a time can be communicated.  
The statement  

<pre>Channel_Int ! 3
</pre>

specifies that the process wants to communicate the integer value 3.  
**Note** The same statement can be used to both send and receive the Integer value 3.

The statement  

<pre>Channel_Int ! id
</pre>

specifies that the process wants to communicate the integer value of variable id .  
**Note** No new variable is introduced for id.<a name="Communication-of-underspecified-data"></a>

### Communication of underspecified data[¶](#Communication-of-underspecified-data)

Let Channel_Int be a channel over which one integer at a time can be communicated.  
The statement  

<pre>Channel_Int ? x
</pre>

specifies that the process wants to communicate an integer value.  
After communication, the value will be stored in the newly introduced variable x.  
**Note** The same statement can be used to both send and receive the underspecified variable x.

The actual communication might be  

<pre>Channel_Int ! 3
</pre>

<pre>Channel_Int ! -3
</pre>

or  

<pre>Channel_Int ! -12345678901234567890
</pre>

<a name="Communication-of-constrained-data"></a>

### Communication of constrained data[¶](#Communication-of-constrained-data)

Let Channel_Int be a channel over which one integer at a time can be communicated.  
The statement  

<pre>Channel ? x [[ x >= 0 ]]
</pre>

specifies that the process wants to communicate a constrained integer value.  
The integer value is constrained to be larger than or equal to zero.  
The value is stored in the newly introduced variable x.

The actual communication might be  

<pre>Channel ! 3
</pre>

but NOT a negative value like  

<pre>Channel ! -3
</pre>

<a name="Communication-of-multiple-data-items"></a>

### Communication of multiple data items[¶](#Communication-of-multiple-data-items)

Let Channel_Int_List be a channel over which one integer and one list at a time can be communicated.  
The statement  

<pre>Channel_Int_List ! id ? list [[ isCstr_Int(list) ]]
</pre>

specifies that the process wants to communicate the following two data: the value of the variable id and a non-empty list.