<a name="State-Automaton-Definition"></a>

# State Automaton Definition[¶](#State-Automaton-Definition)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>stautDef  
</td>

<td>"STAUTDEF" procName "[" neChannelDeclList? "]" "(" neVarDeclList? ")" exitDecl? "::=" stautItem+ "ENDDEF"  
</td>

</tr>

<tr>

<td>stautItem  
</td>

<td>( "STATE" stateName  
| "VAR" neVarDeclList?  
| "INIT" stateName update*  
| "TRANS" transition+  
)  
</td>

</tr>

<tr>

<td>transition  
</td>

<td>statename "->" [Communications](Communications) ("{" update "}")* "->" stateName  
</td>

</tr>

<tr>

<td>update  
</td>

<td>varName+ ":=" [valExpr](ValExpr)  
</td>

</tr>

<tr>

<td>neChannelsDeclList  
</td>

<td>channelsDecl (";" channelsDecl)*  
</td>

</tr>

<tr>

<td>channelsDecl  
</td>

<td>neChannelNameList ("::" neTypeNameList)?  
</td>

</tr>

<tr>

<td>neChannelNameList  
</td>

<td>channelName ("," channelName)*  
</td>

</tr>

<tr>

<td>neVarDeclList  
</td>

<td>varsDecl (";" varsDecl)*  
</td>

</tr>

<tr>

<td>varsDecl  
</td>

<td>neVarNameList "::" typeName  
</td>

</tr>

<tr>

<td>neVarNameList  
</td>

<td>varName ("," varName)*  
</td>

</tr>

<tr>

<td>exitDecl  
</td>

<td>"EXIT" ("::" neTypeNameList)?  
</td>

</tr>

<tr>

<td>neTypeNameList  
</td>

<td>typeName ("#" typeName)*  
</td>

</tr>

<tr>

<td>procName  
</td>

<td>[SmallId](SmallId)  
</td>

</tr>

<tr>

<td>channelName  
</td>

<td>[CapsId](CapsId)  
</td>

</tr>

<tr>

<td>typeName  
</td>

<td>[CapsId](CapsId)  
</td>

</tr>

<tr>

<td>varName  
</td>

<td>[SmallId](SmallId)  
</td>

</tr>

<tr>

<td>stateName  
</td>

<td>[SmallId](SmallId)  
</td>

</tr>

</tbody>

</table>

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

Define a state automaton.

A state automaton is a structure consisting of states, state variables, and transitions. A transition specifies how to go from one state to another state, while performing communications and updating the state variables. INIT specifies the initial state and the initial values of the state variables. The header of a state automaton definition is analogous to the header of [Process Definition PROCDEF](ProcDefs).

<a name="Examples"></a>

## Examples[¶](#Examples)

<pre>STAUTDEF  check1000 [ Add :: Int;  Sum :: Int;  Success ]  ( start_value :: Int )
 ::=
      STATE  state0, state1
      VAR    sum :: Int
      INIT   state0 { sum := start_value }

      TRANS  state0  ->  Add ? x    [[ x >= 0      ]]  { sum := sum + x }      ->  state1
             state1  ->  Sum ! sum  [[ sum <= 1000 ]]  { }                     ->  state0
             state1  ->  Success    [[ sum >= 1000 ]]  { sum := start_value }  ->  state0
ENDDEF
</pre>

The state automaton `check1000` specifies a system with two state `state0` and `state1`, one state variable `sum`, and three transitions. The system adds positive integer inputs until the sum is at least 1000, upon which it outputs `success`. Then it restarts the process starting with `sum := start_value`. Note the non-determinism when the `sum` is exactly equal to 1000.