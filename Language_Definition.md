<a name="Language-Definition"></a>

# Language Definition[¶](#Language-Definition)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

<table>

<tbody>

<tr>

<td>specification  
</td>

<td>( [modelDefs](ModelDefs)  
| [cnectDefs](CnectDefs)  
| [chanDefs](ChanDefs)  
| [typeDefs](TypeDefs)  
| [constDefs](ConstDefs)  
| [funcDefs](FuncDefs)  
| [procDefs](ProcDefs)  
| [stautDef](StautDef)  
) *  
</td>

</tr>

</tbody>

</table>

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

A specification contains zero or more definitions.  
A [Model Definition](ModelDefs) is needed to step a TorXakis model.  
A [Model Definition](ModelDefs) and [Connection Definition](CnectDefs) are minimally needed to test a SUT or simulate a system using a TorXakis model.