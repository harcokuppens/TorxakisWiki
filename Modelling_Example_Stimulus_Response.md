<a name="Modelling-Example-Stimulus-Response"></a>

# Modelling Example Stimulus Response[¶](#Modelling-Example-Stimulus-Response)

<a name="System-Under-Test"></a>

## System Under Test[¶](#System-Under-Test)

We want to test our [java program](Java_program) 'StimulusResponse.java'  

    /**********************************************************************************************
     Communication via stream-mode socket;
     *********************************************************************************************/
    import java.net.*;
    import java.io.*;
    import java.util.concurrent.*;

    public class StimulusResponse
    {
        public static void main(String[] args)
        {  
            try
            {
                // instantiate a socket for accepting a connection
                ServerSocket servsock = new ServerSocket(7890);

                // wait to accept a connection request and a data socket is returned
                Socket sock = servsock.accept();

                // get an input stream for reading from the data socket
                InputStream inStream = sock.getInputStream();

                // create a BufferedReader object for text line input
                BufferedReader sockin = new BufferedReader(new InputStreamReader(inStream));

                // get an output stream for writing to the data socket
                OutputStream outStream = sock.getOutputStream();

                // create a PrinterWriter object for character-mode output
                PrintWriter sockout = new PrintWriter(new OutputStreamWriter(outStream));

                // read a line from the data stream: the stimulus
                sockin.readLine();
                // send a line to the data stream: the response
                sockout.print("\n");
                sockout.flush();

                // TorXakis targets embedded systems and servers that typically don't terminate
                TimeUnit.SECONDS.sleep(10);
            }
            catch (Exception ex) { ex.printStackTrace(); }
        }
    }

As described in the code above, our System Under Test (SUT) communicates with the outside world by sending and receiving lines, i.e., strings terminated by a line feed, over a socket at port 7890.  
When TorXakis and the SUT will run on the same machine, i.e., localhost, the [SUT Definition](SUTDEF.html) in TorXakis is as follows:  

<pre>SUTDEF Sut ::=
    SUT IN     In  :: String
    SUT OUT    Out :: String

    SOCK IN    In   HOST "localhost"  PORT 7890
    SOCK OUT   Out  HOST "localhost"  PORT 7890
ENDDEF
</pre>

<a name="Specification"></a>

## Specification[¶](#Specification)

We specify that the system has two channels, one incoming for the stimulus and one outgoing for the response.  
Furthermore, we specify that first a stimulus and then a response are communicated.  
The [Specification Definition](SpecDefs.html) in TorXakis is as follows:  

<pre>SPECDEF Spec ::=
    CHAN IN    Stimulus
    CHAN OUT   Response

    BEHAVIOUR  
        Stimulus >-> Response
ENDDEF
</pre>

<a name="Adapter"></a>

## Adapter[¶](#Adapter)

To match the specification with the System Under Test, we define the following adapter:  

<pre>ADAPDEF Adap ::=
    CHAN IN    Stimulus    SUT IN    In  :: String
    CHAN OUT   Response    SUT OUT   Out :: String

    MAP IN     Stimulus    ->   In ! "" 
    MAP OUT    Out ? s     ->   Response
ENDDEF
</pre>

<a name="Model-Based-Testing"></a>

## Model Based Testing[¶](#Model-Based-Testing)

1.  Start the SUT: run the [Java program](Java_program) in a command window.  

    <pre> java StimulusResponse
    </pre>

2.  Start TorXakis: run the [TorXakis](TorXakis) with the StimulusResponse model describe above in another command window.  

    <pre> torxakis StimulusResponse.txs
    </pre>

3.  Set the Specification, Adapter, and SUT: In TorXakis type the following commands  

    <pre> spec Spec
     adap Adap
     sut Sut
    </pre>

4.  Test the SUT: In TorXakis type the following command  

    <pre> test 3
    </pre>

    TorXakis will perform the two communication actions, Stimulus and Response,  
    observe that the system under test communicates no additional output,  
    and finally conclude  

    <pre> PASS
    </pre>

<a name="Errorneous-SUT"></a>

### Errorneous SUT[¶](#Errorneous-SUT)

1.  Start the errorneous SUT: run the errorneous [Java program](Java_program) 'StimulusNoResponse.java' in a command window.  

    <pre> java StimulusNoResponse
    </pre>

2.  Start TorXakis: run the [TorXakis](TorXakis) with the StimulusResponse model describe above in another command window.  

    <pre> torxakis StimulusResponse.txs
    </pre>

3.  Set the Specification, Adapter, and SUT: In TorXakis type the following commands  

    <pre> spec Spec
     adap Adap
     sut Sut
    </pre>

4.  Test the erroneous SUT: In TorXakis type the following command  

    <pre> test 3
    </pre>

    TorXakis will perform the first communication action: Stimulus.  
    TorXakis will observe that the expected second communication action, Response, doesn't occur,  
    and conclude  

    <pre> FAIL
    </pre>