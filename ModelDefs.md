<a name="Model-Definitions"></a>

# Model Definitions[¶](#Model-Definitions)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>modelDef  
</td>

<td>"MODELDEF" modelName "::=" "CHAN" "IN" neChannelNameList? "CHAN" "OUT" neChannelNameList? "BEHAVIOUR" [processBehaviour](ProcessBehaviour) "ENDDEF"  
</td>

</tr>

<tr>

<td>neChannelNameList  
</td>

<td>channelName ("," channelName)*  
</td>

</tr>

<tr>

<td>modelName  
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

</tbody>

</table>

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

Define a model, with its input and output channels for external communication, and the definition of its behaviour. Input and output channels shall be defined in a channel definition [CHANDEF](ChanDefs).

<a name="Examples"></a>

## Examples[¶](#Examples)

The following model synchronizes using the [Synchronized Operator ||](Synchronized_Operator) a specification with a sequence.  

<pre>MODELDEF  Model ::=
      CHAN IN    A, B
      CHAN OUT   C, D 
      BEHAVIOUR 
         specification[A,B,C,D](Nil) || sequence[A,B,C,D]()
ENDDEF
</pre>