# Test Purpose Definitions

## Syntax

|Term | Explanation |
|---|---|
| purpDef | "PURPDEF" purpName "::=" "CHAN" "IN" neChannelNameList? "CHAN" "OUT" neChannelNameList? goals "ENDDEF" |
| neChannelNameList | channelName ("," channelName)* |
| modelName | [CapsId](CapsId) |
| channelName | [CapsId](CapsId) |
| goals | goal (, goal)* |
| goal | "GOAL" goalName "::=" [processBehaviour](ProcessBehaviour) |

## Semantics

Define a Test Purpose, with its input and output channels for external communication, and the definition of the behaviour to be tested.
Input and output channels shall be defined in a [channel definition](ChanDefs).

## Examples

A test purpose for a [Model Definition](ModelDefs) like:
```
PROCDEF modelBehaviour [ In :: InType ; Out :: OutType ] () ::=
    -- Model behaviour definition
ENDDEF

MODELDEF Model ::=
    CHAN IN    In
    CHAN OUT   Out

    BEHAVIOUR  
        modelBehaviour[In, Out]()
ENDDEF
```
can be:
```
PROCDEF processBehaviour [ In :: InType ; Out :: OutType ] ( inputValue :: Int) HIT ::=
    -- Process behaviour definition
ENDDEF

PURPDEF TestPurpose ::=
    CHAN IN    In
    CHAN OUT   Out

    GOAL purposeGoal1 ::= processBehaviour [In,Out] (1)
    GOAL purposeGoal2 ::= processBehaviour [In,Out] (2)
ENDDEF

```
