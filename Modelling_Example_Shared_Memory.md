<a name="Modelling-Example-Shared-Memory"></a>

# Modelling Example - Shared Memory[¶](#Modelling-Example-Shared-Memory)

How can we model shared memory, i.e., a global variable, in TorXakis?  
For simplicity, we will assume that the shared memory stores a value of [Data Type](Data_Type) Int.

<a name="Basic-Solution"></a>

## Basic Solution[¶](#Basic-Solution)

We define a process _memory_ to manage the shared memory.  
In the basic solution, this _memory_ process has two communication channels of [Data Type](Data_Type) Int: Read and Write.  
This _memory_ process is a recursive process, with one parameter: the current memory value.  
The process  
either provides the current value over the Read channel,  
or receives a new value over the Write channel.  
Since reading a value from memory doesn't change its value, after reading, the _memory_ process is instantiated with the current memory value.  
Since writing a value to memory changes its value, after writing, the _memory_ process is instantiated with the new value.  

<pre>PROCDEF memory [ Read, Write :: Int ] ( value :: Int ) ::=
        Read  ! value     >->  memory [Read, Write]( value )
    ##
        Write ? newValue  >->  memory [Read, Write]( newValue )
ENDDEF
</pre>

The _memory_ process is used as follows, with initial value 0:  

<pre>PROCDEF usage [ Read, Write :: Int ] ( ) ::=
        memory [ Read, Write ] ( 0 )
    |[ Read, Write ]|
        someProcesses [ Read, Write ] ()
ENDDEF
</pre>

where someProcesses contains read statements like

<table>

<tbody>

<tr>

<td>

<pre>Read ? x 
</pre>

</td>

<th>_Read the value in memory_  
</th>

</tr>

<tr>

<td>

<pre>Read ! 0
</pre>

</td>

<th>_Read the memory when the value is zero_  
</th>

</tr>

<tr>

<td>

<pre>Read ? x [[ x < 0 ]]
</pre>

</td>

<th>_Read the memory when the value is less then zero_  
</th>

</tr>

</tbody>

</table>

and write statements like

<table>

<tbody>

<tr>

<td>

<pre>Write ! 123
</pre>

</td>

<th>_Write 123 to memory_  
</th>

</tr>

<tr>

<td>

<pre>Write ? x
</pre>

</td>

<th>_Write an arbritrary value to memory_  
</th>

</tr>

<tr>

<td>

<pre>Write ? x [[ x > 123 ]]
</pre>

</td>

<th>_Write a value larger than 123 to memory_  
</th>

</tr>

</tbody>

</table>

See the example ReadWriteConflict for more details.

<a name="Advanced-Solution"></a>

## Advanced Solution[¶](#Advanced-Solution)

In the advanced solution, the _memory_ process has only one communication channel: Memory.  
This communication channel has as [Data Type](Data_Type) MemoryAccess which is defined as follows  

<pre>TYPEDEF MemoryAccess ::= Read  { value :: Int }
                       | Write { newValue :: Int }
ENDDEF
</pre>

The _memory_ process is still a recursive process, with one parameter: the current memory value.  
The process  
either provides the Read constructor with the current value over the Memory channel,  
or receives a Write constructor with the new value over the Memory channel.  
Since reading a value from memory doesn't change its value, after reading, the _memory_ process is instantiated with the current memory value.  
Since writing a value to memory changes its value, after writing, the _memory_ process is instantiated with the new value.  

<pre>PROCDEF memory [ Memory :: MemoryAccess ] ( value :: Int ) ::=
        Memory ! Read ( value )     >->  memory [Memory](value)
    ##
        Memory ? x [[ isWrite(x) ]] >->  memory [Memory](newValue(x))
ENDDEF
</pre>

The _memory_ process is used as follows, with initial value 0:  

<pre>PROCDEF usage [ Memory :: MemoryAccess ] ( ) ::=
        memory [ Memory ] ( 0 )
    |[ Memory ]|
        someProcesses [ Memory ] ()
ENDDEF
</pre>

where someProcesses contains read statements like

<table>

<tbody>

<tr>

<td>

<pre>Memory ? x [[ isRead(x) ]]
</pre>

</td>

<th>_Read the value in memory_  
</th>

</tr>

<tr>

<td>

<pre>Memory ! Read(0)
</pre>

</td>

<th>_Read the memory when the value is zero_  
</th>

</tr>

<tr>

<td>

<pre>Memory ? x [[ isRead(x) /\ (value(x) < 0) ]]
</pre>

</td>

<th>_Read the memory when the value is less then zero_  
</th>

</tr>

</tbody>

</table>

and write statements like

<table>

<tbody>

<tr>

<td>

<pre>Memory ! Write(123)
</pre>

</td>

<th>_Write 123 to memory_  
</th>

</tr>

<tr>

<td>

<pre>Memory ? x [[ isWrite(x) ]]
</pre>

</td>

<th>_Write an arbritrary value to memory_  
</th>

</tr>

<tr>

<td>

<pre>Memory ? x [[ isWrite(x) /\ (newValue(x) > 123) ]]
</pre>

</td>

<th>_Write a value larger than 123 to memory_  
</th>

</tr>

</tbody>

</table>

The advanced solution has a better performance in TorXakis.