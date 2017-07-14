<a name="Parallel-Operator"></a>

# Parallel Operator[¶](#Parallel-Operator)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

[processBehaviour](ProcessBehaviour) "|||" [processBehaviour](ProcessBehaviour)

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

process1 ||| process2

The processes 1 and 2 run independently in parallel.  
No synchronization between the processes is required.  
Of course, the processes might communicate with each other.

When both processes have exited then parallel composition exits

<a name="Examples"></a>

## Examples[¶](#Examples)

The statement  

<pre>ChannelId1 ? x ||| ChannelId2 ? y
</pre>

describes the process that can, in any order, do the following two things:

*   Communicate variable x over ChannelId1
*   Communicate variable y over ChannelId2

The process ends when both communications have occurred.

In other words, the following three behaviours can be observed:

<table>

<tbody>

<tr>

<td>First 1 then 2  
</td>

<td>ChannelId1 ? x [>->](Sequence_Operator) ChannelId2 ? y  
</td>

</tr>

<tr>

<td>First 2 then 1  
</td>

<td>ChannelId2 ? y [>->](Sequence_Operator) ChannelId1 ? x  
</td>

</tr>

<tr>

<td>Synchronously  
</td>

<td>ChannelId1 ? x [|](Synchronous_Operator) ChannelId2 ? y  
</td>

</tr>

</tbody>

</table>