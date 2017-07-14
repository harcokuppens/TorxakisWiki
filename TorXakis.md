<a name="TorXakis"></a>

# TorXakis[¶](#TorXakis)

TorXakis uses a [specification](Language_Definition) to test a System-Under-Test (SUT) or simulate a System.

A TorXakis [specification](Language_Definition) contains three parts:

*   [Model](ModelDefs) - Composition of processes instances that models the behavior of the SUT
*   [Adapter](AdapDefs.html) - Map abstract specification on actual SUT interfaces.
*   [Connection](CnectDefs) - Describe external interfaces to SUT or of simulated system.

TorXakis uses the Input-Output Conformance (IOCO) theory to  
check the actual against the specified externally observable behaviour.

TorXakis is a command line tool that takes zero or more [specification](Language_Definition) files as input.  
![](/attachments/download/1623/CommandLineTool.jpg)  
TorXakis has many [commands](Commands): the command 'help' provides a short summary of all [commands](Commands).

A TorXakis Editor for the Eclipse IDE is provided as a plug-in. See [Eclipse](Eclipse).

Copyright © 2015-2016 TNO and Radboud University  
All rights reserved.