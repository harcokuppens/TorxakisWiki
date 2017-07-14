<a name="Channel-Definition"></a>

# Channel Definition[¶](#Channel-Definition)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>chanDefs  
</td>

<td>"CHANDEF" chanDefName "::=" neChannelsDeclList "ENDDEF"  
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

<td>neTypeNameList  
</td>

<td>typeName ("#" typeName)*  
</td>

</tr>

<tr>

<td>chanDefName  
</td>

<td>[CapsId](CapsId)  
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

</tbody>

</table>

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

Define all channels that are used on the highest level in the TorXakis-file, i.e., in model definitions ([MODELDEF](ModelDefs)) and in connection definitions ([CNECTDEF](CnectDefs)). For each channel the types of messages communicated via that channel are defined. At the CHANDEF level, channels do not have an I/O-direction yet; I/O is added on the level of [MODELDEF](ModelDefs) and [CNECTDEF](CnectDefs).

<a name="Examples"></a>

## Examples[¶](#Examples)

The definition  

<pre>CHANDEF Channels
    ::=
        Action :: Operation;
        Input  :: Int # Int;
        Result :: Int
ENDDEF
</pre>

defines three channels: `Action`, `Input`, and `Result`, with messages of types `Operation`, (`Int` x `Int`), and `Int`, respectively.