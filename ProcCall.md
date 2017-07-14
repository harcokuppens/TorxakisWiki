<a name="Process-Call"></a>

# Process Call[¶](#Process-Call)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>procCall  
</td>

<td>procName "[" neChannelNameList? "]" "(" neValExprList? ")"  
</td>

</tr>

<tr>

<td>neChannelNameList  
</td>

<td>channelName ("," channelName)*  
</td>

</tr>

<tr>

<td>neValExprList  
</td>

<td>[valExpr](ValExpr) ("," [valExpr](ValExpr))*  
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

</tbody>

</table>

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

Call a user defined process, defined with [PROCDEF](ProcDefs).  
Channel name must refer to a predefined channel name.