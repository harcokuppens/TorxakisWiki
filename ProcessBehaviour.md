<a name="ProcessBehaviour"></a>

# ProcessBehaviour[¶](#ProcessBehaviour)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>processBehaviour  
</td>

<td>  processBehaviour "[>>>](Enable_Operator)" processBehaviour  
| processBehaviour "[>>>](Enable_Operator)" "ACCEPT" ("?" [varDecl](VarDecl) | "!" [valExpr](ValExpr) )* IN processBehaviour NI  
| processBehaviour "[[>>](Disable_Operator)" processBehaviour  
| processBehaviour "[[><](Interrupt_Operator)" processBehaviour  
| processBehaviour "[||](Synchronized_Operator)" processBehaviour  
| processBehaviour "[|||](Parallel_Operator)" processBehaviour  
| [processBehavioursSynchronizedOnChannels](Synchronized_Channels_Operator)  
| processBehaviour "[##](Choice_Operator)" processBehaviour  
| [condition](Condition) "[=>>](Guard_Operator)" processBehaviour  
| [communications](Communications) ("[>->](Sequence_Operator)" processBehaviour)?  
| [STOP](STOP)  
| [procCall](ProcCall)  
| [letProcessBehaviour](LET)  
| [hideChannelsInProcessBehaviour](HIDE)  
| "(" processBehaviour ")"  
</td>

</tr>

</tbody>

</table>

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

<a name="Examples"></a>

## Examples[¶](#Examples)