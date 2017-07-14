<a name="HIDE"></a>

# HIDE[¶](#HIDE)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>hideProcessBehaviour  
</td>

<td>"HIDE" "[" neChannelsDeclList? "]" "IN" [processBehaviour](ProcessBehaviour) "NI"  
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

Hide a set of channels.  
Hence, the environment can not synchronize over these channels.

<a name="Examples"></a>

## Examples[¶](#Examples)

The statement  

<pre>HIDE [ Channel ] IN
    A >-> Channel >-> C
|[ Channel ]|
    B >-> Channel >-> D
NI
</pre>

synchronizes the two processes on an internal Channel.  
The externally observable communication behaviour is equal to  

<pre>(A ||| B) >>> (C ||| D)
</pre>

or in words, communications on channels A and B occur before communications on channels C and D.

The statement  

<pre>HIDE [ Channel :: Int ] IN
    A ? x  >-> Channel ! (x + 10)
|[ Channel ]|
    Channel ? y >-> B ! (y + 20)
NI
</pre>

synchronizes the two processes on an internal Channel of type Int.  
The externally observable communication behaviour is equal to  

<pre> A ? x >-> B ! (x + 30)
</pre>