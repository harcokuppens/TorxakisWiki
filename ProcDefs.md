<a name="Proccess-Definitions"></a>

# Proccess Definitions[¶](#Proccess-Definitions)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>procDefs  
</td>

<td>"PROCDEF" procDef (";" procDef )* "ENDDEF"  
</td>

</tr>

<tr>

<td>procDef  
</td>

<td>procName "[" neChannelDeclList? "]" "(" neVarDeclList? ")" exitDecl? "::=" [processBehaviour](ProcessBehaviour)  
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

<td>"EXIT" neTypeNameList?  
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

</tbody>

</table>

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

Define processes.

<a name="Examples"></a>

## Examples[¶](#Examples)

The statement  

<pre>PROCDEF myProcess [ Chan :: Int # Int ] ( n :: Int ) ::=
    Chan ! n ? x >-> myProcess [ Chan ] ( n+1 )
ENDDEF
</pre>

defines the recursive process, myProcess.  
This process communicates a tuple of two integers on the channel Chan.  
The first integer of the tuple is incremented with each communication.  
The second integer of the tuple is unconstrained.