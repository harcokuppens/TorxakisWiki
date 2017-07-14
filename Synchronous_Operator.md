<a name="Synchronous-Operator"></a>

# Synchronous Operator[¶](#Synchronous-Operator)

<a name="Syntax"></a>

## Syntax[¶](#Syntax)

[communications](Communications) "|" [communications](Communications)

<a name="Semantics"></a>

## Semantics[¶](#Semantics)

communication1 | communication2

Both communication1 and communication2 are execute at the same time.  
These communications can only occur when both have a set of matching processes to communicate with.

<a name="Examples"></a>

## Examples[¶](#Examples)

The statement  

<pre>ChannelId1 ? x | ChannelId2 ? y
</pre>

desribe the process that at the same time

*   communicates variable x over ChannelId1 and
*   communicates variable y over ChannelId2

For this, both communications must, of course, be possible.

The statement  

<pre>CreateAccount ? account | CreateUser ? id ? pw1 ? pw2 [[ (id(account) == id) /\ (pw(account) == pw1) /\ (pw(account) == pw2) ]]
</pre>

desribe the process that at the same time creates an account using a data type and by the triplet of id, password, and confirm password.  
Thus the synchronous operator is used to link the different levels of abstraction.