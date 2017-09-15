<a name="Installation"></a>
# Installation

The release can be downloaded from [releases](https://github.com/TorXakis/TorXakis/releases).
For windows an installer is provided.

<a name="Development"></a>
# Development

To obtain the development version:
* Clone the github archive.
* Install stack
* Run `stack install` (takes over 1 hour) or `stack install --fast`
* Install a SMT solver 
* Configure TorXakis to use the installed SMT Solver.

The installation of a SMT Solver is described in more details in the following subsection.

<a name="Install-SMT-Solver"></a>

## Install SMT Solver

TorXakis uses a satisfiability modulo theories (SMT) solver to handle the data constraints. For more info on SMT and the available SMT solvers see e.g. [wiki on satisfiability modulo theories](https://en.wikipedia.org/wiki/Satisfiability_modulo_theories).  
TorXakis requires at least one installed SMT Solver.  
However, since your TorXakis model determines how the SMT solver will be used, and each SMT solver has its strengths and weaknesses,  
you might want to install multiple SMT Solvers and experiment with which SMT solver to use.

The TorXakis requirements for the SMT solver are

1.  Support the [SMT-LIB version 2.5 standard](http://smtlib.cs.uiowa.edu/papers/smt-lib-reference-v2.5-r2015-06-28.pdf)
2.  Support algebraic datatypes (as in the [SMT-LIB version 2.6 standard draft](http://smtlib.cs.uiowa.edu/papers/smt-lib-reference-v2.6-draft-1.pdf))
3.  Support [strings](http://cvc4.cs.nyu.edu/wiki/Strings) (which will probably be included in the SMT-LIB version 3.0 standard)

Since, some of the features TorXakis requires are not yet standardized, official releases of SMT solvers supporting all required features do not exist.

TorXakis has been extensively tested with development versions of the SMT solvers [CVC4](http://cvc4.cs.nyu.edu/) and [Z3](https://github.com/Z3Prover/z3/wiki).

*   The development version of CVC4 for multiple different platforms can be downloaded from [http://cvc4.cs.nyu.edu/downloads/](http://cvc4.cs.nyu.edu/downloads/) in the right column.
*   The development version of Z3 for multiple different platforms can be downloaded from [https://github.com/Z3Prover/bin/tree/master/nightly](https://github.com/Z3Prover/bin/tree/master/nightly)
