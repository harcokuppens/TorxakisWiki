<a name="Connection-Definitions"></a>

# Connection Definitions[¶](#Connection-Definitions)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>cnectDefs  
</td>

<td>"CNECTDEF" cnectDef (";" cnectDef )* "ENDDEF"  
</td>

</tr>

<tr>

<td>cnectDef  
</td>

<td>cnectName "::=" cnectType cnectItem*  
</td>

</tr>

<tr>

<td>cnectType  
</td>

<td>"CLIENTSOCK" | "SERVERSOCK"  
</td>

</tr>

<tr>

<td>cnectItem  
</td>

<td>  "CHAN" "OUT" channelName "HOST" hostName "PORT" portNumber  
| "CHAN" "IN" channelName "HOST" hostName "PORT" portNumber  
| "ENCODE" channelName "?" [varDecl](VarDecl) "->" "!" [valExpr](ValExpr)  
| "DECODE" channelName "!" [valExpr](ValExpr) "<-" "?" [varDecl](VarDecl)  
</td>

</tr>

<tr>

<td>cnectName  
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

<td>hostName  
</td>

<td>[String](Data_Type)  
</td>

</tr>

<tr>

<td>portNumber  
</td>

<td>[Int](Data_Type)  
</td>

</tr>

</tbody>

</table>

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

Define connections with outside world, and the mapping from abstract TorXakis channels to the concrete outside-world connections. Currently, only socket connections (of type String) are supported. A socket has a hostname, such as "localhost", and a portNumber.

<a name="Examples"></a>

## Examples[¶](#Examples)

The statement  

<pre>CNECTDEF  Sut
    ::=
        CLIENTSOCK

        CHAN  OUT  Action                         HOST "localhost"  PORT 7890
        ENCODE     Action  ? opn              ->  ! toString(opn)

        CHAN  IN   Result ::                      HOST "localhost"  PORT 7890
        DECODE     Result  ! fromString(s)   <-   ? s

ENDDEF
</pre>

specifies a socket connection, called `Sut`, to the outside world where

*   _TorXakis_ is the client side;
*   there is one outgoing and one incoming channel, `Action` and `Result`, respectively;
*   the types of messages communicated on channels `Action` and `Result` can be found in the corresponding channel definition CHANDEF;
*   the predefined function `toString` is used to map a message of channel `Action` to a string for the socket (localhost,7890);
*   the predefined function `fromString` is used to map a string from socket (localhost,7890) to a message on channel `Result`;
*   both channels the outside world are accessed via one socket on the same machine, i.e., localhost on portNumber 7890; one channel is `IN` and one is `OUT`.  
    Note that
*   `OUT` and `IN` are viewed from the point of view of _TorXakis_, i.e., if this CNECTDEF specifies the connection to the system under test (SUT) then `OUT` is the input for the SUT, and `IN` is the output of the SUT;
*   each channel must map uniquely (bijectively) to a triple (`IN`/`OUT`,hostName,portNumber).

The statement  

<pre>CNECTDEF  Sim
    ::=
        SERVERSOCK

        CHAN  IN   Action :: Operation            HOST "localhost"  PORT 7890
        DECODE     Action  ! fromString(s)   <-   ? s

        CHAN  OUT  Result :: Int                  HOST "localhost"  PORT 7890
        ENCODE     Result  ? result           ->  ! toString(result)

ENDDEF
</pre>

specifies a socket connection, called `Sim`, to the outside world, where

*   _TorXakis_ is the server side;
*   there is one incoming and one outgoing channel, `Action` and `Result`, respectively;
*   the channel mappings are the inverse of the previous connection definition, implying that this connection can be used for a simulator simulating the same model as used above for testing.