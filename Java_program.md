<a name="Java-program"></a>

# Java program[¶](#Java-program)

Many Systems Under Test in the TorXakis examples are java programs.  
One needs the [Java Development Kit](http://www.oracle.com/technetwork/java/javase/downloads/index.html "JDK") to play with these examples.

<a name="Byte-code"></a>

## Byte code[¶](#Byte-code)

To obtain byte code from the source code inside a *.java file, one needs the Java Compiler.  
Assuming the Java Compiler is on the system's PATH, the compilation command for the java source code file 'example.java' is  

<pre>   javac example.java
</pre>

When compilation is successful, the directory now contains a file containing the byte code called 'example.class'.<a name="Execution"></a>

## Execution[¶](#Execution)

Assuming the Java Program is on the system's PATH  
and the current directory, i.e., '.', is on the CLASSPATH,  
the command is to run the byte code is  

<pre>   java example
</pre>

<a name="Arguments"></a>

### Arguments[¶](#Arguments)

Some examples need arguments.  
Assuming that the program needs one argument, the command to run the byte code is  

<pre>   java example 1234
</pre>