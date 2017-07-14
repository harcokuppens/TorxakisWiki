<a name="Installation"></a>

# Installation[¶](#Installation)

The installation consists of 3 steps:

*   Download TorXakis
*   Install SMT Solver
*   Configure TorXakis

Each of these steps is described in more details in the following three subsections.

<a name="Download-TorXakis"></a>

## Download TorXakis[¶](#Download-TorXakis)

Download TorXakis from [http://torxakis.esi.nl/downloads](http://torxakis.esi.nl/downloads).  
Save the executable on your computer  
![](/attachments/download/1624/SaveAs.jpg)

Note you might get the following warning  
![](/attachments/download/1625/NotCommonlyDownloadedCouldHarm.jpg)

<a name="Install-SMT-Solver"></a>

## Install SMT Solver[¶](#Install-SMT-Solver)

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

<a name="Configure-TorXakis"></a>

## Configure TorXakis[¶](#Configure-TorXakis)

On all platforms, TorXakis interacts with the SMT solver via the 'smt.bat' file.  
Create a 'smt.bat' file that points to an installed, executable and accessible SMT solver.  
Make sure that 'smt.bat' file is readable and executable.

All examples below assume that the SMT solver is on the system's PATH.  
More info on [how to set the PATH variable in Windows](http://www.computerhope.com/issues/ch000549.htm)

<a name="Z3-Examples-of-smtbat-file"></a>

### Z3 Examples of 'smt.bat' file[¶](#Z3-Examples-of-smtbat-file)

Z3 on Microsoft Windows and Cygwin  

<pre>   @z3.exe -smt2 -in
</pre>

Z3 on Linux  

<pre>   z3.exe -smt2 -in
</pre>

<a name="CVC4-Examples-of-smtbat-file"></a>

### CVC4 Examples of 'smt.bat' file[¶](#CVC4-Examples-of-smtbat-file)

Since the date is encoded in the name of the development versions of CVC4,  
all CVC4 examples assume you downloaded the development version of 2016 03 02.  
When you downloaded a development version with another date,  
your 'smt.bat' file should of course point to that version of CVC4.  
Consequently, the date part of the name should be different than in the examples.

CVC4 on Microsoft Windows and Cygwin, assuming you downloaded the development version of 2016 03 02  

<pre>   @cvc4-2016-03-02-win32-opt.exe --lang=smt --incremental --strings-exp --fmf-fun-rlv --uf-ss-fair
</pre>

CVC4 on Linux, assuming you downloaded the development version of 2016 03 02  

<pre>   cvc4-2016-03-02-x86_64-linux-opt --lang=smt --incremental --strings-exp --fmf-fun-rlv --uf-ss-fair
</pre>

**Note** You might run into a [CVC4 Performance Bug](http://cvc4.cs.nyu.edu/bugs/show_bug.cgi?id=706).  
In that case, the performance might improve when you use the following flags for CVC4 in your 'smt.bat' file  

<pre>  --lang=smt --tear-down-incremental 1 --strings-exp --fmf-fun-rlv --uf-ss-fair
</pre>