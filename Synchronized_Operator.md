<a name="Synchronized-Operator"></a>

# Synchronized Operator[¶](#Synchronized-Operator)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

[processBehaviour](ProcessBehaviour) "||" [processBehaviour](ProcessBehaviour)

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

process1 || process2

process1 and process2 are executed while synchronized over all channels, including [EXIT](EXIT).

When both processes have exited then the synchronized composition exits.

<a name="Examples"></a>

## Examples[¶](#Examples)

<a name="Communication"></a>

### Communication[¶](#Communication)

The statement  

<pre>    Channel1_Int ? x 
||
    Channel1_Int ? y
</pre>

describes the process with two sub processes that are synchronized over all channels.  
Since both sub processes want to communicate over Channel1_Int, communication is possible.  
After that communication has occurred x and y contain the same value, and  
since both communications have exited, the synchronized composition exits.<a name="Constrained-communication"></a>

### Constrained communication[¶](#Constrained-communication)

The statement  

<pre>    Channel1_Int ? x [[ x >= 10 ]]
||
    Channel1_Int ? y [[ y <= 20 ]]
</pre>

describes the process with two sub processes that are synchronized over all channels.  
Since both sub processes want to communicate over Channel1_Int and  
both constraints can be simultaneously satisfied, communication is possible.  
After that communication has occurred x and y contain the same value in the range [10,20]  
and, since both communications have exited, the synchronized composition exits.<a name="Deadlock"></a>

### Deadlock[¶](#Deadlock)

The statement  

<pre>    Channel1_Int ? x 
||
    Channel2_Int ? y
</pre>

describes the process with two sub processes that are synchronized over all channels.  
Since one sub process wants to communicate the variable x over Channel1_Int and  
the other sub process wants to communicate the variable y over another channel, Channel2_Int,  
communication under the synchronization constraint is not possible, i.e., the process is in [deadlock](https://en.wikipedia.org/wiki/Deadlock).