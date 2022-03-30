Scheduling Lab
===============
*Operating Systems, Spring 2020*


**Gayeon Park**

***DISCLAIMER: I would like to make it clear that the following lab specifications are taken from the lab instructon distributed to our OS class by Professor Yan Shvartzshnaider. If you have any questions or want to see the original lab instructions, please refer to the [linked file.](https://github.com/gayeon97/Operating-Systems-Lab-02/blob/main/Scheduling_lab_original_instructions.pdf)***

----
### Overview ###

For this Java program, we are simulating scheduling to see how time requried for process is allocated, depending on the scheduling algorithm and the request patterns. 


Here, a process is characterized by just four non-negative integers: A, B, C and M.
- A is the arrival time of the process. 
- B is used for calculating the CPU Burst time for a process, using the randomOS(B) function. 
- C is the total CPU time needed by the process. 
- M is used for calculating the IO Burst time for a process.


Using these four information about the process, we are trying to replicate how an operating system would schedule multiple processes to run in real life.

----
### Program Layout ###

A process execution consists of computation alternating with I/O. We will refer to these as the **CPU bursts** and **I/O bursts** throughout the description.


To calculate **CPU burst times**, following simplifying assumption is made:
* For each process, the CPU burst times are uniformly distributed random integers (or UDRIs for short) in the interval **(0, B]**. 
* To obtain a UDRI ***t*** in some interval **(0, U]**, the function **randomOS(U)(( is used. 
    * **randomOS(U)** is a simple function that reads a random non-negative integer X from a file named ***random-numbers*** (in the src directory) and returns the value 1 + (X mod U). 
    * ***random-numbers*** is the file supplied by the professor that contains a large number of random non-negative integers. The purpose of standardizing the random numbers is so that all correct programs will produce the same answers.
* So the next CPU burst is randomOS(B). If the value returned by randomOS(B), is larger than the total CPU time remaining, set the next CPU burst to the remaining time.


The **I/O burst time** for a process is its preceding CPU burst time multiplied by M.


You are to read a file describing n processes (i.e., n quadruples of numbers) and then simulate the n processes
until they all terminate. The way to do this is to keep track of the state of each process (e.g., ready, running,
blocked) and advance time, making any state transitions needed. At the end of the run you first print an
identification of the run including the scheduling algorithm used, any parameters (e.g. the quantum for RR),
and the number of processes simulated



- - - -
#### Instructions to run the code ####

The Scheduling.java program reads its input from a file, whose name is given as a command line argument. The program send its output to the screen as System.out in Java.

The program accepts an optional "-verbose" flag. If present, it precedes the file name. When "-verbose" is given, the program produce detailed output that's helpful for debugging. 

The program also accepts an optional "-show-random" flag. If present, it precedes the file name. When "-show-random" is given, the program produce detailed output along with the random number chosen each time. This is a more verbose version.

Three possible invocations of the program are:
<program-name> <input-filename>
<program-name> --verbose <input-filename>
<program-name> --show-random <input-filename>




To run on crackle1 on cims.nyu.edu server, please first go to crackle1, where the folder "Lab2" is uploaded. Once you are at crackle1, please navigate to the Scheduling.java file like the following:
     Change directory to Lab2, then change directory to src.
     or like: cd Lab2/src

Inside of the "src" folder, Scheduling.java file is located.
To execute the program using the contents of a text file, please type the name of the java file (Scheduling), the name of any flag (-verbose or -show-random), if any, and the name of the input file. 

Type the below instruction into the Terminal to compile the Scheduling.java program.
When compiling, you have to make sure that you are compiling both the Scheduling.java file and the Process.java file because Scheduling.java uses an instance of Process class.

ONE IMPORTANT THING TO NOTE: the input files you are testing the Scheduling.java file against MUST be in the same folder as the Scheduling.java file
So Scheduling.java file, Process.java file, and whatever input file you want to test HAVE to be in the same folder (namely, src folder inside of the Lab2 folder).

### Compiling
```
javac Scheduling.java Process.java
```

### Running
To execute the code, type following:
```
java Scheduling input-number                    //for a regular output

or 
java Scheduling --verbose input-number          //for a detailed output

or
java Scheduling --show-random input-number      //for a even more detailed output


```


Once again, the input text files and Scheduling.java file are located in same folder, "src" inside of Lab2 folder. 
If you want to execute the Scheduling.java file with the contents of "input-7" file, do the following: 

### Compiling
```
javac Scheduling.java Process.java
```

### Running
To execute the code, type following:
```
java Scheduling input-7                     //for a regular output

or
java Scheduling --verbose input-7           //for a detailed output

or
java Scheduling --show-random input-7       //for a even more detailed output


```
