<a name="Synchronized-Channels-Operator"></a>

# Synchronized Channels Operator[¶](#Synchronized-Channels-Operator)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>processesSynchronizedChannels  
</td>

<td>[processBehaviour](ProcessBehaviour) "|[" channelName ("," channelName)* "]|" [processBehaviour](ProcessBehaviour)  
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

process1 |[ ChanA, ChanB ]| process2

process1 and process2 are executed while synchronized over the channels A and B, and [EXIT](EXIT).  
When both processes have exited then the synchronized channels operator exits.

Each channel name must refer to a channel defined in the scope of this operator.

<a name="Example"></a>

## Example[¶](#Example)

Describe the externally observable communication behaviour of the following instance of a synchronized channels operator.  

<pre>    ChannelId2 ? x
|[ ChannelId1, ChannelId2 ]|
    (
        ChannelId1 ? k 
    ||| 
        ChannelId2 ? l
    |||
        ChannelId3 ? m 
    )
</pre>

*   No communication over ChannelId1 is possible, since only one of the two processes that are synchronized over ChannelId1 and ChannelId2 wants to communicate over ChannelId1.
*   Communication over ChanneldId2 is possible, since both processes that are synchronized over ChannelId1 and ChannelId2 wants to communicate over ChannelId2\. The variables x and l will have the same value after communication has occurred.
*   Communication over ChannelId3 is possible, since one of the two processes that are synchronized over ChannelId1 and ChannelId2 wants to communicate over another channel: ChannelId3\. Of course, another process is needed to communicate with over ChannelId3.