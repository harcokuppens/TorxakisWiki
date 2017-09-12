# Modelling Example - Lucky People
We want to test our [java program](Java_program) [LuckyPeople.java](https://github.com/TorXakis/TorXakis/blob/develop/examps/LuckyPeople/sut/LuckyPeople.java).
This program gets as input a sequence of persons and determines if those people are lucky. The detailed behaviour of this program can be found at the [assignment page](Modelling_Example_Lucky_People).
All the files used in this example can be found at [the example folder](https://github.com/TorXakis/TorXakis/tree/develop/examps/LuckyPeople).
## Types
We start with defining the types that are needed to model This program's (our System Under Test - SUT) behaviour.
The input of our SUT are persons with first name, last name, sex, day of birth and month of birth. Let's [define the types](TypeDefs) needed:
```
TYPEDEF Sex ::= Male | Female ENDDEF

TYPEDEF Person ::=
    Person { sex :: Sex 
           ; firstName, lastName :: String 
           ; dayOfBirth, monthOfBirth :: Int
           }
ENDDEF
```
## Channels
Our SUT receives a sequence of persons and determines if those people are lucky. Let's [define the channels](ChanDefs) needed for modelling this behaviour:
```
CHANDEF Channels ::=  In   :: Person
                    ; Out  :: Bool
ENDDEF
```
## Procedure
In our [model definition](ModelDefs), we need a way to define the behaviour of determining whether a Person is lucky or not.
Let's implement a [procedure definition](ProcDefs) for this:

`PROCDEF luckyPeople [ In :: Person; Out :: Bool ] ( last :: Sex; n :: Int ) ::=`

"*In*" and "*Out*" are our channels, while "*last*" and "*n*" are the parameters of this procedure. We added the parameters because we want to keep track of how many people of the same sex have been before current person, since it is one of the ways of determining whether a person is lucky.

We want to test our SUT with inputs that make sense, so we should tell TorXakis what makes a Person input sensible. Let's add a function as a constraint for the input:

`In ? p [[ isValid_Person(p) ]]`

We're going to implement "*isValid_Person()*" function later. For now let's focus on the process.

Next, we want to communicate to the output whether this person is lucky or not. Let's use another function for it:

`>-> Out ! isLuckyPerson (p, last, n)`

"*>->*" is the [Sequence Operator](Sequence_Operator). It means that after the process on the left has communicated, the described process behaviour on the right is exposed.

After communicating the result to the output channel, it's time to go back and wait for next input. There's one catch, though: As we said above, we should keep track of how many people with the current sex have been processed so far.

```
>->(
            ( [[ sex(p) == last ]] =>> luckyPeople[In,Out] ( sex(p), n+1 ) )
        ##
            ( [[ sex(p) <> last ]] =>> luckyPeople[In,Out] ( sex(p), 1 ) )
    )
```

"*##*" is the [Choice Operator](Choice_Operator). It means that either one of those processes are executed, but never both.
"*=>>*" is the [Guard Operator](Guard_Operator). The process behaviour on the right is executed only if the expression on the left of it is *true*.

So the [process definition](ProcDefs) of Lucky People looks like this:
```
PROCDEF luckyPeople [ In :: Person; Out :: Bool ] ( last :: Sex; n :: Int ) ::=
        In ? p [[ isValid_Person(p) ]] 
    >-> Out ! isLuckyPerson (p, last, n) 
    >->(
                ( [[ sex(p) == last ]] =>> luckyPeople[In,Out] ( sex(p), n+1 ) )
            ##
                ( [[ sex(p) <> last ]] =>> luckyPeople[In,Out] ( sex(p), 1 ) )
        )
ENDDEF
```
Now we can use this [procedure definition](ProcDefs) in our [Model](ModelDefs).
## Model
The model should specify that the system uses two channels, one incoming for the Person and one outgoing for the result.
It should also specify that "*luckyPeople*" process is executed with the input and this process's output is communicated to the out channel as this model's behaviour.
```
MODELDEF Model ::=
    CHAN IN    In
    CHAN OUT   Out

    BEHAVIOUR
        luckyPeople[In, Out](Male,0)   -- first sex choice is arbitrary
ENDDEF
```

## Functions
We're not done yet. We need to implement the functions that our "*luckyPeople*" process uses.
### isValid_Person
We want to validate the input in order to make sure that it represents data of a person that can actually exist.
```
FUNCDEF isValid_Person (p :: Person) :: Bool ::=
       strinre (firstName(p), REGEX ('[A-Z][a-z]*'))
    /\ strinre (lastName(p), REGEX ('[A-Z][a-z]*'))
    /\ (1 <= dayOfBirth(p)) /\ (dayOfBirth(p) <= 31)
    /\ (1 <= monthOfBirth(p)) /\ (monthOfBirth(p) <= 12)
ENDDEF
```
"*strinre*" function checks whether the given string matches the given Regular Expression. First two lines ensure that only the inputs with Capital case first name and surname are valid.
### isLuckyPerson
To make the "*luckyPerson*" process cleaner, we decided to extract the logic of determining the person's luckiness into a separate function. Here it goes:
```
FUNCDEF isLuckyPerson (p :: Person; last :: Sex; n :: Int) :: Bool ::=
       ( ( sex(p) <> last ) /\ ( n >= 5 ) )                             -- change of sex after 5 of others?
    \/ ( at(firstName(p), 0 ) == at(lastName(p), 0 ) )                  -- same first character in first and last name?
    \/ ( dayOfBirth(p) == monthOfBirth(p) )                             -- same value for day and month of birth?
ENDDEF
```

## SUT Connection
Our SUT communicates with the outside world by sending and receiving lines i.e. strings terminated by a line feed, over a socket at port 7777.
The SUT also expects one person per input line, fields separated by "@" sign. So we should make sure that the Person type in our TorXakis model is properly serialized before being communicated to the SUT. The return value from the SUT should also be deserialized into proper data structure (Bool).
Assuming that TorXakis and the SUT run on the same machine (localhost) the [SUT connection](CnectDefs) can be defined in TorXakis as follows:
```
CONSTDEF separator :: String ::= "@" ENDDEF

CNECTDEF  Sut ::=
    CLIENTSOCK

    CHAN  OUT  In                   HOST "localhost"  PORT 7777
    ENCODE     In  ? p              ->  ! toString(sex(p))        ++ separator ++
                                          firstName(p)            ++ separator ++
                                          lastName(p)             ++ separator ++
                                          toString(dayOfBirth(p)) ++ separator ++
                                          toString(monthOfBirth(p))

    CHAN  IN   Out                  HOST "localhost"  PORT 7777
    DECODE     Out  ! fromString(s) <-  ? s
ENDDEF

```
## Model Based Testing
1.  Start the SUT: run the [Java program](Java_program) in a command window.

`$> java LuckyPeople`

2.  Start TorXakis: run the [TorXakis](TorXakis) with the LuckyPeople model described above in another command window.

`$> torxakis LuckyPeople.txs`

3.  Set the Model and SUT for testing: In TorXakis type the following commands:

`tester Model Sut`

4.  Test the SUT: In TorXakis type the following command:

`test 50`

TorXakis will create *valid* random Person data, pass it to SUT and receive the output at each step, as long as the SUT responds as expected or the number of test steps are reached, then finally conclude:
```
TXS >>  .....1: IN: Act { { ( In, [ Person(Male,"W","Phz",22,9) ] ) } }
TXS >>  .....2: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  .....3: IN: Act { { ( In, [ Person(Male,"Pa","Hrd",2,2) ] ) } }
TXS >>  .....4: OUT: Act { { ( Out, [ True ] ) } }
...
TXS >>  ....47: IN: Act { { ( In, [ Person(Female,"Pk","Pq",3,11) ] ) } }
TXS >>  ....48: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  ....49: IN: Act { { ( In, [ Person(Female,"Pp","La",31,1) ] ) } }
TXS >>  ....50: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  PASS
```
### Testing with predetermined input
We can also tell TorXakis to use predetermined input data for testing. For this, we can make use of [Test Purpose](TestPurpose)s. We'll define a process that communicates Person data and resulting boolean at each step, then we'll add as many steps as we want for our predetermined input, and then we'll use this process in our Test Purpose.
Let's define that process and write some test data with expected results:
```
PROCDEF examples [ In :: Person; Out :: Bool ] () HIT ::=
        In ! Person( Male, "Mickey", "Mouse", 13, 1 )
    >-> Out ! True
    >-> In ! Person( Male, "Donald", "Duck", 13, 3 )
    >-> Out ! True
    >-> In ! Person( Male, "Luuk", "Laar", 24, 12 )
    >-> Out ! True
    >-> In ! Person( Female, "Shakira", "Ripoll", 2, 2 )
    >-> Out ! True
    >-> In ! Person( Male, "Michael", "Buble", 9, 9 )
    >-> Out ! True
    >-> In ! Person( Female, "Imke", "Laar", 7, 7 )
    >-> Out ! True
    >-> In ! Person( Male, "Huey", "Duck", 17, 10 )
    >-> Out ! False
    >-> In ! Person( Male, "Dewey", "Duck", 17, 10 )
    >-> Out ! True
    >-> In ! Person( Male, "Louie", "Duck", 17, 10 )
    >-> Out ! False
    >-> In ! Person( Male, "Mickey", "Mouse", 13, 1 )
    >-> Out ! True
    >-> In ! Person( Male, "Donald", "Duck", 13, 3 )
    >-> Out ! True
    >-> In ! Person( Female, "April", "Duck", 15, 5 )
    >-> Out ! True
    >-> In ! Person( Female, "Beatrix", "Oranje", 31, 1 )
    >-> Out ! False
    >-> In ! Person( Female, "Maxima", "Zorreguieta", 17, 5 )
    >-> Out ! False
    >-> In ! Person( Female, "Amalia", "Oranje", 7, 12 )
    >-> Out ! False
    >-> In ! Person( Female, "Alexia", "Oranje", 26, 6 )
    >-> Out ! False
    >-> In ! Person( Female, "Ariane", "Oranje", 10, 4 )
    >-> Out ! False
    >-> In ! Person( Male, "Willem", "Oranje", 27, 4 )
    >-> Out ! True
ENDDEF
```
Mind the **HIT** keyword on first line. This keyword is necessary in order to use it as a **Goal** in the Test Purpose definition below.

Now we should make sure the generated input for our SUT is synchronized with what this process communicates to the channels. Let's define the Test Purpose for this:
```
PURPDEF PurposeExamples ::=
    CHAN IN    In 
    CHAN OUT   Out
    
    GOAL examples ::= examples [In,Out] ()
ENDDEF
```
Defining the Goal of the Test Purpose with *examples* process effectively forces the Persons in *examples* process to be communicated through *In* channel to *Model* and expects the output in *Out* channel to match whatever is defined in *examples* process.

Now we can use this Test Purpose to test SUT with our predetermined inputs.

1.  Start the SUT: run the [Java program](Java_program) in a command window.

`$> java LuckyPeople`

2.  Start TorXakis: run the [TorXakis](TorXakis) with the LuckyPeople model described above in another command window.

`$> torxakis LuckyPeople.txs`

3.  Set the Model and SUT for testing: In TorXakis type the following commands:

`tester Model PurposeExamples Sut`

4.  Test the SUT with predetermined inputs. We have 18 persons, which means we need 36 steps. But TorXakis occasionally adds some Quiescence steps in between, so let's run TorXakis for 50 steps instead. In TorXakis type the following command:

`test 50`

```
TXS >>  .....1: IN: Act { { ( In, [ Person(Male,"Mickey","Mouse",13,1) ] ) } }
TXS >>  .....2: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  .....3: OUT: No Output (Quiescence)
TXS >>  .....4: IN: Act { { ( In, [ Person(Male,"Donald","Duck",13,3) ] ) } }
TXS >>  .....5: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  .....6: OUT: No Output (Quiescence)
TXS >>  .....7: IN: Act { { ( In, [ Person(Male,"Luuk","Laar",24,12) ] ) } }
TXS >>  .....8: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  .....9: OUT: No Output (Quiescence)
TXS >>  ....10: IN: Act { { ( In, [ Person(Female,"Shakira","Ripoll",2,2) ] ) } }
TXS >>  ....11: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  ....12: OUT: No Output (Quiescence)
TXS >>  ....13: IN: Act { { ( In, [ Person(Male,"Michael","Buble",9,9) ] ) } }
TXS >>  ....14: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  ....15: IN: Act { { ( In, [ Person(Female,"Imke","Laar",7,7) ] ) } }
TXS >>  ....16: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  ....17: OUT: No Output (Quiescence)
TXS >>  ....18: IN: Act { { ( In, [ Person(Male,"Huey","Duck",17,10) ] ) } }
TXS >>  ....19: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ....20: OUT: No Output (Quiescence)
TXS >>  ....21: IN: Act { { ( In, [ Person(Male,"Dewey","Duck",17,10) ] ) } }
TXS >>  ....22: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  ....23: IN: Act { { ( In, [ Person(Male,"Louie","Duck",17,10) ] ) } }
TXS >>  ....24: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ....25: OUT: No Output (Quiescence)
TXS >>  ....26: IN: Act { { ( In, [ Person(Male,"Mickey","Mouse",13,1) ] ) } }
TXS >>  ....27: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  ....28: OUT: No Output (Quiescence)
TXS >>  ....29: IN: Act { { ( In, [ Person(Male,"Donald","Duck",13,3) ] ) } }
TXS >>  ....30: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  ....31: IN: Act { { ( In, [ Person(Female,"April","Duck",15,5) ] ) } }
TXS >>  ....32: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  ....33: IN: Act { { ( In, [ Person(Female,"Beatrix","Oranje",31,1) ] ) } }
TXS >>  ....34: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ....35: OUT: No Output (Quiescence)
TXS >>  ....36: IN: Act { { ( In, [ Person(Female,"Maxima","Zorreguieta",17,5) ] ) } }
TXS >>  ....37: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ....38: OUT: No Output (Quiescence)
TXS >>  ....39: IN: Act { { ( In, [ Person(Female,"Amalia","Oranje",7,12) ] ) } }
TXS >>  ....40: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ....41: OUT: No Output (Quiescence)
TXS >>  ....42: IN: Act { { ( In, [ Person(Female,"Alexia","Oranje",26,6) ] ) } }
TXS >>  ....43: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ....44: IN: Act { { ( In, [ Person(Female,"Ariane","Oranje",10,4) ] ) } }
TXS >>  ....45: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ....46: IN: Act { { ( In, [ Person(Male,"Willem","Oranje",27,4) ] ) } }
TXS >>  ....47: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  ....48: OUT: No Output (Quiescence)
TXS >>  Goal examples: Hit
TXS >>  PASS
```
### Lucky based on gender
One of the situations that makes a person lucky is when that person's sex is different from previous 5 people. Even though our predetermined inputs cover this case, this criteria comes with implied cases that:

- if _less than 5_ previous persons are of the other sex then the current person **is not lucky**
- if _more than 5_ previous persons are of the other sex then the current person **is lucky**

Covering all these cases with predetermined input is not only not practical but also defeats the purpose of using TorXakis for Model Based Testing. We should make TorXakis to test these situations while still generating test data randomly. We can do it by using a Test Purpose that only enforces the sex of generated Person data and leaves the rest to be generated by TorXakis.

Here is such a [Process Definition](ProcDefs) that can be used here:
```
PROCDEF increasingRows [ In :: Person; Out :: Bool ] ( length, pos :: Int; s :: Sex ) HIT ::=
        In ? p [[ sex(p) == s]]
    >-> Out ? b
    >-> (
                [[pos == 1]] =>> (
                                        [[ isMale(s) ]] =>> increasingRows [In,Out] (length, length, Female)
                                    ##
                                        [[ isFemale(s) ]] =>> increasingRows [In,Out] (length+1, length+1, Male)
                                 )
            ##
                [[pos>1]] =>> increasingRows [In,Out] (length, pos-1, s)
         )
ENDDEF
```
This process enforces repeating the same sex for a number of times, for both sexes, then increments the repetition number and repeats. There's no upper limit defined for the repetition number, so as long as we run TorXakis the generated persons' sexes will be like this: *MFMMFFMMMFFFMMMMFFFF*...

Let's write a Test Purpose that uses *incresingRows* process to manipulate test data generated by TorXakis:
```
PURPDEF PurposeIncreasingRows ::=
    CHAN IN    In 
    CHAN OUT   Out
    
    GOAL increasingRows ::= increasingRows [In,Out] (1,1,Male)
ENDDEF
```
Now we can use this Test Purpose to test SUT for various "Lucky by gender" cases.

1.  Start the SUT: run the [Java program](Java_program) in a command window.

`$> java LuckyPeople`

2.  Start TorXakis: run the [TorXakis](TorXakis) with the LuckyPeople model described above in another command window.

`$> torxakis LuckyPeople.txs`

3.  Set the Model and SUT for testing: In TorXakis type the following commands:

`tester Model PurposeIncreasingRows Sut`

4.  Test the SUT with random data, ensuring coverage of "Lucky by gender" cases. Let's say we test for repetitions of 1 to 10 i.e.:

- Female after 1 Male
- Male after 1 Female
- Female after 2 Males
- Male after 2 Females

...

- Female after 9 Males
- Male after 9 Females
- Female after 10 Males
- Male after 10 Females

This means we have (10*11)/2=55 cases for each gender, 2 steps (In, Out) per case means 110 steps and for both genders including last switch from 10F-to-M **221 steps** in total. But TorXakis occasionally adds some Quiescence steps in between, so let's run TorXakis for 300 steps instead. In TorXakis type the following command:

`test 300`

```
TXS >>  .....1: OUT: No Output (Quiescence)
TXS >>  .....2: IN: Act { { ( In, [ Person(Male,"Ac","Arv",13,5) ] ) } }
TXS >>  .....3: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  .....4: OUT: No Output (Quiescence)
TXS >>  .....5: IN: Act { { ( In, [ Person(Female,"B","D",19,7) ] ) } }
TXS >>  .....6: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  .....7: OUT: No Output (Quiescence)
TXS >>  .....8: IN: Act { { ( In, [ Person(Male,"D","H",29,7) ] ) } }
TXS >>  .....9: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ....10: OUT: No Output (Quiescence)
TXS >>  ....11: IN: Act { { ( In, [ Person(Male,"F","O",30,1) ] ) } }
TXS >>  ....12: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ....13: OUT: No Output (Quiescence)
TXS >>  ....14: IN: Act { { ( In, [ Person(Female,"A","D",2,3) ] ) } }
TXS >>  ....15: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ....16: OUT: No Output (Quiescence)
TXS >>  ....17: IN: Act { { ( In, [ Person(Female,"C","Pxd",3,12) ] ) } }
TXS >>  ....18: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ....19: OUT: No Output (Quiescence)
...
TXS >>  ...255: IN: Act { { ( In, [ Person(Male,"T","B",31,10) ] ) } }
TXS >>  ...256: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ...257: IN: Act { { ( In, [ Person(Female,"B","D",29,8) ] ) } }
TXS >>  ...258: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  ...259: IN: Act { { ( In, [ Person(Female,"Yj","L",31,2) ] ) } }
TXS >>  ...260: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ...261: IN: Act { { ( In, [ Person(Female,"N","H",9,11) ] ) } }
TXS >>  ...262: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ...263: IN: Act { { ( In, [ Person(Female,"Pdp","A",5,7) ] ) } }
TXS >>  ...264: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ...265: IN: Act { { ( In, [ Person(Female,"Lh","H",30,11) ] ) } }
TXS >>  ...266: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ...267: IN: Act { { ( In, [ Person(Female,"Z","P",31,10) ] ) } }
TXS >>  ...268: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ...269: OUT: No Output (Quiescence)
TXS >>  ...270: IN: Act { { ( In, [ Person(Female,"V","C",18,9) ] ) } }
TXS >>  ...271: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ...272: IN: Act { { ( In, [ Person(Female,"Hbx","H",30,1) ] ) } }
TXS >>  ...273: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  ...274: IN: Act { { ( In, [ Person(Female,"D","K",18,1) ] ) } }
TXS >>  ...275: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ...276: OUT: No Output (Quiescence)
TXS >>  ...277: IN: Act { { ( In, [ Person(Female,"Wp","Pkx",29,12) ] ) } }
TXS >>  ...278: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ...279: OUT: No Output (Quiescence)
TXS >>  ...280: IN: Act { { ( In, [ Person(Male,"G","Ahagpa",14,4) ] ) } }    <<== This is a Male after 10 Females
TXS >>  ...281: OUT: Act { { ( Out, [ True ] ) } }                            <<== He's lucky based on gender
TXS >>  ...282: OUT: No Output (Quiescence)
TXS >>  ...283: IN: Act { { ( In, [ Person(Male,"F","A",30,8) ] ) } }
TXS >>  ...284: OUT: Act { { ( Out, [ False ] ) } }
...
TXS >>  ...299: IN: Act { { ( In, [ Person(Male,"M","P",23,6) ] ) } }
TXS >>  ...300: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  PASS
```
Two points for attention:

- In our **increasingRows** process we didn't have to tell TorXakis that after switching the gender the next person has to be lucky on unlucky; we just enforced a certain sex to be assigned to current person. TorXakis knows what each person's luckiness output should be depending on the **Model**.
- Since we do not manipulate other data of persons, it is possible that a person which should be lucky based on gender is actually lucky due to some other condition. This does nullify our aim, but we ignore such cases for now. What we are sure about, though, is that a person who comes after 5+ people from the opposite gender is *always* lucky. On the other hand, it's possible to ensure that such people are *only* lucky by gender with a more complex *increasingRows* process.

## XML-Based communication
TorXakis is capable of XML-based communication with SUT's. We don't have an SUT which does that but TorXakis can help us there, too: TorXakis can also act as a simulator which behaves based on a model.
Let's [define another connection](CnectDefs) and a simulator, both of which use XML-based communication.
```
CNECTDEF  Xut ::=
    CLIENTSOCK

    CHAN  OUT  In                   HOST "localhost"  PORT 7777
    ENCODE     In  ? p              ->  ! toXml(p)
    
    CHAN  IN   Out                  HOST "localhost"  PORT 7777
    DECODE     Out  ! fromXml(s)    <-  ? s
ENDDEF

CNECTDEF  Xim ::=
    SERVERSOCK

    CHAN IN   In                    HOST "localhost"  PORT 7777
    DECODE    In ! fromXml(s)       <-  ? s

    CHAN OUT  Out                   HOST "localhost"  PORT 7777
    ENCODE    Out ? b               ->  ! toXml(b)
ENDDEF
```
Let's test our simulator.
1.  Start TorXakis as simulator: run the [TorXakis](TorXakis) with the LuckyPeople model described above in a command window.

`$> torxakis LuckyPeople.txs`

2.  Set the Model and Simulator for testing: In TorXakis type the following commands:

`simulator Model Xim`

TorXakis will hang, waiting for a tester to connect.

3.  Start another TorXakis instance as the Tester: run the [TorXakis](TorXakis) with the LuckyPeople model described above in another command window.

`$> torxakis LuckyPeople.txs`

4.  Set the Model and Sut for testing: In TorXakis type the following commands:

`tester Model Xut`

As soon as you enter this command, you'll see that Simulator also responds:

`TXS >>  Simulator started`

5.  Simulator works a bit slower than the Java implementation. To prevent unexpected delays to cause false negatives, increase delta times of tester.
In the command window of the tester, input following commands:
```
param param_Sim_deltaTime 4000
param param_Sut_deltaTime 4000
```

6.  Start Simulator with a high number of steps, in order to not run out of steps before testing is finished:

`sim 30`

7.  Test the Simulator: In TorXakis type the following command:

`test 10`

Now you'll see the tester TorXakis instance happily testing the simulator:
```
TXS >>  .....1: IN: Act { { ( In, [ Person(Male,"K","D",2,1) ] ) } }
TXS >>  .....2: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  .....3: IN: Act { { ( In, [ Person(Male,"B","L",8,12) ] ) } }
TXS >>  .....4: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  .....5: IN: Act { { ( In, [ Person(Male,"Phdppab","S",19,2) ] ) } }
TXS >>  .....6: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  .....7: IN: Act { { ( In, [ Person(Male,"Addp","Pz",17,8) ] ) } }
TXS >>  .....8: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  .....9: IN: Act { { ( In, [ Person(Female,"Hfhnad","Ln",4,8) ] ) } }
TXS >>  ....10: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  PASS
```

And simulator TorXakis instance acting as the SUT:
```
TXS >>  .....1: OUT: No Output (Quiescence)
TXS >>  .....2: OUT: No Output (Quiescence)
TXS >>  .....3: IN: Act { { ( In, [ Person(Male,"K","D",2,1) ] ) } }
TXS >>  .....4: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  .....5: OUT: No Output (Quiescence)
TXS >>  .....6: IN: Act { { ( In, [ Person(Male,"B","L",8,12) ] ) } }
TXS >>  .....7: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  .....8: IN: Act { { ( In, [ Person(Male,"Phdppab","S",19,2) ] ) } }
TXS >>  .....9: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ....10: OUT: No Output (Quiescence)
TXS >>  ....11: IN: Act { { ( In, [ Person(Male,"Addp","Pz",17,8) ] ) } }
TXS >>  ....12: OUT: Act { { ( Out, [ False ] ) } }
TXS >>  ....13: IN: Act { { ( In, [ Person(Female,"Hfhnad","Ln",4,8) ] ) } }
TXS >>  ....14: OUT: Act { { ( Out, [ True ] ) } }
TXS >>  ....15: OUT: No Output (Quiescence)
...
TXS >>  ....30: OUT: No Output (Quiescence)
TXS >>  PASS
```
