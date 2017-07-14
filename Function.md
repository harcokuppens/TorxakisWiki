<a name="Function"></a>

# Function[¶](#Function)

TorXakis has predefined, implicitly defined [TYPEDEF](TypeDefs), and user defined functions.

<a name="Predefined-Functions"></a>

## Predefined Functions[¶](#Predefined-Functions)

TorXakis has the following predefined functions; grouped by predefined data type to enhance readability.

<a name="Bool"></a>

### Bool[¶](#Bool)

<table>

<tbody>

<tr>

<th>function  
</th>

<th>description  
</th>

</tr>

<tr>

<td>==(a,b :: Bool) ::= Bool  
</td>

<td>Equals Infix operator  
</td>

</tr>

<tr>

<td><>(a,b :: Bool) ::= Bool  
</td>

<td>Not Equals Infix operator  
</td>

</tr>

<tr>

<td>toString(a :: Bool) ::= String  
</td>

<td>Bool to String function  
</td>

</tr>

<tr>

<td>fromString(s :: String) ::= Bool  
</td>

<td>Bool from String function  
</td>

</tr>

<tr>

<td>isFalse(b :: Bool) :: Bool  
</td>

<td>is False function -- TODO: decide remove since is equal to not?  
</td>

</tr>

<tr>

<td>isTrue(b :: Bool) :: Bool  
</td>

<td>is True function -- TODO: decide remove since is equal to its input b?  
</td>

</tr>

<tr>

<td>not(b :: Bool) :: Bool  
</td>

<td>not function  
</td>

</tr>

<tr>

<td>/\(a,b :: Bool) :: Bool  
</td>

<td>and Infix Operator  
</td>

</tr>

<tr>

<td>\/(a,b :: Bool) :: Bool  
</td>

<td>or Infix Operator  
</td>

</tr>

<tr>

<td>\|/(a,b :: Bool) :: Bool  
</td>

<td>xor Infix Operator  
</td>

</tr>

<tr>

<td>=>(a,b :: Bool) :: Bool  
</td>

<td>implies Infix Operator  
</td>

</tr>

</tbody>

</table>

<a name="Int"></a>

### Int[¶](#Int)

<table>

<tbody>

<tr>

<th>function  
</th>

<th>description  
</th>

</tr>

<tr>

<td>==(a,b :: Int) ::= Bool  
</td>

<td>Equals Infix operator  
</td>

</tr>

<tr>

<td><>(a,b :: Int) ::= Bool  
</td>

<td>Not Equals Infix operator  
</td>

</tr>

<tr>

<td>toString(a :: Int) ::= String  
</td>

<td>Int to String function  
</td>

</tr>

<tr>

<td>fromString(s :: String) ::= Int  
</td>

<td>Int from String function  
</td>

</tr>

<tr>

<td>+(i :: Int) :: Int  
</td>

<td>Prefix Operator +  
</td>

</tr>

<tr>

<td>-(i :: Int) :: Int  
</td>

<td>Prefix Operator -  
</td>

</tr>

<tr>

<td>abs(i :: Int) :: Int  
</td>

<td>Absolute value function  
</td>

</tr>

<tr>

<td>+(a,b :: Int) ::= Int  
</td>

<td>Addition Infix operator  
</td>

</tr>

<tr>

<td>-(a,b :: Int) ::= Int  
</td>

<td>Substraction Infix operator  
</td>

</tr>

<tr>

<td>*(a,b :: Int) ::= Int  
</td>

<td>Multiplication Infix operator  
</td>

</tr>

<tr>

<td>/(a,b :: Int) ::= Int  
</td>

<td>Division Infix operator  
</td>

</tr>

<tr>

<td>%(a,b :: Int) ::= Int  
</td>

<td>Modulo Infix operator  
</td>

</tr>

<tr>

<td><(a,b :: Int) ::= Bool  
</td>

<td>Less Then Infix operator  
</td>

</tr>

<tr>

<td><=(a,b :: Int) ::= Bool  
</td>

<td>Less Equal Infix operator  
</td>

</tr>

<tr>

<td>>=(a,b :: Int) ::= Bool  
</td>

<td>Greater Equal Infix operator  
</td>

</tr>

<tr>

<td>>(a,b :: Int) ::= Bool  
</td>

<td>Greater Then Infix operator  
</td>

</tr>

</tbody>

</table>

<a name="String"></a>

### String[¶](#String)

<table>

<tbody>

<tr>

<th>function  
</th>

<th>description  
</th>

</tr>

<tr>

<td>==(a,b :: String) ::= Bool  
</td>

<td>Equals Infix operator  
</td>

</tr>

<tr>

<td><>(a,b :: String) ::= Bool  
</td>

<td>Not Equals Infix operator  
</td>

</tr>

<tr>

<td>++(a,b :: String) ::= String  
</td>

<td>Concat Infix operator  
</td>

</tr>

<tr>

<td>len(s ::String) :: Int  
</td>

<td>Length of String function  
</td>

</tr>

<tr>

<td>at(s :: String; i :: Int) :: String  
</td>

<td>Character at position i of s.  
The index of position starts at 0\.  
**Note** at is a [partial function](http://cvc4.cs.nyu.edu/wiki/Strings#Partial_Functions).  
To achieve a desirable solution, also guard proper conditions,  
i.e., add assertions to enforce: (i >= 0) /\ (i < len(s))  
</td>

</tr>

</tbody>

</table>

<a name="Regex"></a>

### Regex[¶](#Regex)

<table>

<tbody>

<tr>

<th>function  
</th>

<th>description  
</th>

</tr>

<tr>

<td>strinre(s :: String; r :: Regex) :: Bool  
</td>

<td>Adheres string to regex?  
</td>

</tr>

</tbody>

</table>

<a name="Implicitly-Defined-TYPEDEF-Functions"></a>

## Implicitly Defined [TYPEDEF](TypeDefs) Functions[¶](#Implicitly-Defined-TYPEDEF-Functions)

TorXakis will automatically generate equality, type checking, and accessors functions for the user defined data types.  
**Note** accessor functions associated with a particular constructor are only defined for instances of that constructor.

<a name="Example"></a>

### Example[¶](#Example)

When the user defines  

<pre>TYPEDEF List_Int ::=
      CNil_Int
    | Cstr_Int { head :: Int; tail :: List_Int }
ENDDEF
</pre>

TorXakis defines the equality operator  

<pre>==(a,b :: List_Int) :: Bool
</pre>

the type checking functions (according to the pattern is<constructorName>)  

<pre>isCNil_Int(x :: List_Int) :: Bool
isCstr_Int(x :: List_Int) :: Bool 
</pre>

and the accessor functions  

<pre>head(x :: List_Int) :: Int
tail(x :: List_Int) :: List_Int
</pre>

which satisfy  

<pre>head(Cstr_Int(h,t)) == h
tail(Cstr_Int(h,t)) == t
</pre>

Note that these accessor functions are only defined for instances of the Cstr_Int constructor, i.e.,  
instances of List_Int for which isCstr_Int(x) returns True. head(CNil_Int) and tail(CNil_Int) are thus not defined.

One should guard the usage of accessor functions with the constructor check, using [IF THEN ELSE FI](IteValExpr) .  

<pre>IF isCstr_Int(x) THEN head(x) == 5 ELSE False FI
</pre>

<a name="User-Defined-Functions"></a>

## User Defined Functions[¶](#User-Defined-Functions)

In TorXakis, the user can define functions, including recursive functions, using  
[FUNCDEF](FuncDefs).