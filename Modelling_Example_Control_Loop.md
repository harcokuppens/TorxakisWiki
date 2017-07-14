<a name="Modelling-Example-Control-Loop"></a>

# Modelling Example - Control Loop[¶](#Modelling-Example-Control-Loop)

In this example, we model a control loop.  
The control loop consists out of three processes: produce, measure, and correct.  
Between these processes material and reports are exchanged.  
The three processes repeat the following steps:

*   The produce process makes the material according to specification,
*   The measure process measures the produced material, and
*   The correct process determines corrections based on the differences between specification and measurement.

After briefly introducing the data types and function that are used in this model, we will discuss these three processes in order of simplicity. We will end with the complete control loop model.

<a name="data-types"></a>

## data types[¶](#data-types)

All data types are Cartesian Products of two integers: id and value.  

<pre>TYPEDEF ProduceReport ::= ProduceReport { id, value :: Int } ENDDEF
TYPEDEF MeasureReport ::= MeasureReport { id, value :: Int } ENDDEF
TYPEDEF Correction    ::= Correction    { id, value :: Int } ENDDEF
</pre>

<a name="function"></a>

## function[¶](#function)

The sign function: zero for zero, +1 for all positive numbers, and -1 for all negative numbers.  

<pre>FUNCDEF sgn ( x :: Int ) :: Int ::=
    IF x > 0 
        THEN 1
        ELSE IF x < 0
                THEN -1
                ELSE 0 
             FI
    FI
ENDDEF
</pre>

<a name="correct"></a>

## correct[¶](#correct)

![](/attachments/download/1614/correct.jpg)

Correct is a repeating process of corrections.  
All corrections need two inputs: a production report and a measurement report.  
These reports must of course refer to the same material.  
All corrections result in one output: a correction.  
A correction contains

*   a reference to the material,
*   a value.

The correction value has a relation with the two inputs: the intended and measured values as reported in the production and measurement reports, respectively. This relation is expressed as a constraint between the correction, intended, and measured values.

The behaviour of the correct process is captured in the following recursive TorXakis [Process Definition](ProcDefs):  

<pre>PROCDEF correct [ In_ProduceReport      :: ProduceReport
                ; In_MeasureReport      :: MeasureReport
                ; Out_Correction        :: Correction
                ] ( ) ::=
        In_ProduceReport ? pr | In_MeasureReport ? mr [[ id(pr) == id(mr) ]]
    >-> Out_Correction ? correction [[ ( id(correction) == id(mr) ) /\ ( sgn(value(correction)) == sgn( value(pr) - value(mr) ) )]]
    >-> correct [ In_ProduceReport, In_MeasureReport, Out_Correction ] ( )
ENDDEF
</pre>

<a name="measure"></a>

## measure[¶](#measure)

![](/attachments/download/1615/measure.jpg)

Measure is a repeating process of measurements.  
All measurements need one input: the material to measure.  
All measurements result in two outputs: the measured material and a report containing the measurement results.  
The order of these two outputs is not constrained, i.e.,  

<pre>  Out_Material ||| Out_Report
</pre>

Measure is modelled without buffers for material. So the behaviour of measure with respect to material is the alternating sequence of material coming in and material going out, i.e.,  

<pre>  In_Material >-> Out_Material >-> In_Material >-> Out_Material >-> In_Material >-> ...
</pre>

Incoming material is associated with an identifier. This indentifier not only must be associated with the outgoing material but also be present in the generated report.

The behaviour of the measure process is captured in the following recursive TorXakis [Process Definition](ProcDefs):  

<pre>PROCDEF measure [ In_Material   :: Int
                ; Out_Report    :: MeasureReport
                ; Out_Material  :: Int
                ]( ) ::=
        In_Material ? id
    >-> (
                Out_Report ? mr [[ id(mr) == id ]]
            |||
                (
                        Out_Material ! id 
                    >-> measure [ In_Material, Out_Report, Out_Material ] ()
                )
        )
ENDDEF
</pre>

<a name="produce"></a>

## produce[¶](#produce)

![](/attachments/download/1616/produce.jpg)

Produce is a repeating process of production steps.  
All production steps results in two outputs: material and report.  
The order of these two outputs is not constrained, i.e.,  

<pre>  Out_Material ||| Out_Report
</pre>

In the first production step, only material is needed as input.  
In all other production steps, both material and correction are needed as inputs.  
The order of these two inputs is not constrained, i.e.,  

<pre>  In_Material ||| In_Correction
</pre>

All inputs are required to produce the output. So for all, but the first production step holds  

<pre>  ( In_Material ||| In_Correction ) >>> ( Out_Material ||| Out_Report )
</pre>

Produce is modelled without buffers for material. So the behaviour of produce with respect to material is the alternating sequence of material coming in and material going out, i.e.,  

<pre>  In_Material >-> Out_Material >-> In_Material >-> Out_Material >-> In_Material >-> ...
</pre>

A correction is only obtained after both the production report and the outgoing material are used. To be precise, to obtain a correction both a production report and a measure report are needed. The latter is obtained when the material is measured. This requirement on order can be express as follows:

<pre>  ( Out_Material ||| Out_Report ) >>> In_Correction
</pre>

Incoming material is associated with an identifier. This indentifier not only must be associated with the outgoing material but also be present in the generated report and in the resulting correction.

The behaviour of the produce process is captured in the following TorXakis [Process Definitions](ProcDefs).  
These two processes were needed to capture the difference between the first and all other production steps.  

<pre>PROCDEF produce [ In_Material           :: Int
                ; In_Correction         :: Correction
                ; Out_Material          :: Int
                ; Out_ProduceReport     :: ProduceReport
                ]( ) ::=
        In_Material ? id
    >-> produceLoop [In_Material, In_Correction, Out_Material, Out_ProduceReport] (id)
ENDDEF

PROCDEF produceLoop [ In_Material           :: Int
                    ; In_Correction         :: Correction
                    ; Out_Material          :: Int
                    ; Out_ProduceReport     :: ProduceReport
                    ]( id :: Int ) ::=
    (       
            Out_Material ! id 
        >-> In_Material ? nextId
        >-> EXIT ! nextId
    )
    |[ Out_Material ]|
    (
        (
                ( Out_Material ! id >-> EXIT )
            |||
                ( Out_ProduceReport ? pr [[ id(pr) == id ]] >-> EXIT )
        ) >>>
            ( In_Correction ? correction [[ id (correction) == id ]] >-> EXIT ? nextId :: Int )
    )
    >>> ACCEPT ? nextId :: Int IN
        produceLoop [In_Material, In_Correction, Out_Material, Out_ProduceReport] (nextId)
    NI
ENDDEF
</pre>

<a name="control-loop"></a>

## control loop[¶](#control-loop)

![](/attachments/download/1613/controlloop.jpg)

The behaviour of the control loop process is captured in the following TorXakis [Process Definitions](ProcDefs).  
The three earlier described processes are linked together using their communication channels.  

<pre>PROCDEF model [ In_Material                 :: Int
              ; Correction                  :: Correction
              ; ProduceReport               :: ProduceReport
              ; Material                    :: Int
              ; MeasureReport               :: MeasureReport
              ; Out_Material                :: Int
              ]( ) ::=
        (
                produce [ In_Material, Correction, Material, ProduceReport ] ( )
            |[ Material]|
                measure [ Material, MeasureReport, Out_Material ] ( )
        )
    |[ ProduceReport, MeasureReport, Correction ]|
        correct [ ProduceReport, MeasureReport, Correction ] ( )
ENDDEF
</pre>