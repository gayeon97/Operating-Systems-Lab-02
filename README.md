Scheduling Lab
===============
*Operating Systems, Spring 2020*


**Gayeon Park**

***DISCLAIMER: I would like to make it clear that the following lab specifications are taken from the lab instructon distributed to our OS class by Professor Yan Shvartzshnaider. If you have any questions or want to see the original lab instructions, please refer to the [linked file.](https://github.com/gayeon97/Operating-Systems-Lab-02/blob/main/Scheduling_lab_original_instructions.pdf)***

----
### Overview ###

For this **Scheduling.java program**, we are simulating scheduling to see how time requried for process is allocated, depending on the scheduling algorithm and the request patterns. 


Here, a process is characterized by just four non-negative integers: A, B, C and M.
- A is the arrival time of the process. 
- B is used for calculating the CPU Burst time for a process, using the randomOS(B) function. 
- C is the total CPU time needed by the process. 
- M is used for calculating the IO Burst time for a process.

 
Each of the following scheduling algorithms are simulated:
* FCFS
* RR with quantum 2.
* LCFS (Last come first served. Yes, this is weird.)
* HPRN. Define the denominator to be max(1, running time), to prevent dividing by zero for a job that has yet to be run. Remember that HPRN is non-preemptive.

***For simplicity, we assume that a context switch takes zero time. Hence, the calculations are done only every time unit (e.g., we  assume nothing happens at time 2.5).***

For each scheduling algorithm, there are several runs with different process mixes. 
A mix is a value of ***n*** followed by ***n "A, B, C, M"*** quadruples.
For example, look at the following ***input-1*** file and ***input-2*** file (ignore the comments):
```
// input-1 file. 
// Here, n = 1. We have one "A,B,C,M" quadruples (A = 0, B = 1, C = 5, M = 1)
1 0 1 5 1 about as easy as possible

// input-2 file
// Here, n = 2, so we've two "A,B,C,M" quadruples. (two A=0, B=1, C=5, M=1 groups)
2 0 1 5 1 0 1 5 1 should alternate with FCFS
```

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


Since there are three places where the above specification is not deterministic and different choices can lead to different answers among students, we had to do the following to standardize the answers:
1. A running process can have up to three events occur during the same cycle.
    1. It can terminate (remaining CPU time goes to zero).
    2. It can block (remaining CPU burst time goes to zero).
    3. It can be preempted (e.g., the RR quantum goes to zero).
They must be considered in the above order. For example, if all three occur at one cycle, the process terminates.
2. Many jobs can have the same “priority”: 
    * For example, in RR scheduling algorithm, several jobs can become ready at the same cycle and thus have the same priority. 
    * You must decide in what order they will subsequently be run. These ties are broken by favoring the process with the earliest arrival time A. 
    * If the arrival times are the same for two processes with the same priority, then favor the process that is listed earliest in the input. 
    * **We break ties to standardize answers.** We use this particular rule for two reason. 
        1. First, it does break all ties. 
        2. Second, it is very simple to implement. However, it is not especially wonderful and is not used in practice. 

- - - -
### Input File & Expected Program Output ###

For this Scheduling Lab, a given input file describes n processes (i.e., n quadruples of numbers). 
The **Scheduling.java** program reads the input file and then simulates the ***n*** processes until they all terminate. In such process, we keep track of the state of each process (ready, running, blocked) and simultaneously advance time, making any state transitions that are needed. 


***Although it wasn't a requirement, I have allowed extra command line argument to be put before the name of the input file to produce debugging information optionally. Please refer to the [instructions for the code about the verbose flag](#verbose).*** 

At the end of the run, following things are printed to the system output:
* an identification of the run including the scheduling algorithm used
* any parameters (e.g. the quantum for RR)
* the number of processes simulated


Then for each process, we print: 
* A, B, C, and M
* Finishing time.
* Turnaround time (i.e., finishing time - A).
* I/O time (i.e., time in Blocked state).
* Waiting time (i.e., time in Ready state).


Lastly, the following summary data are printed:
* Finishing time (i.e., when all the processes have finished).
* CPU Utilization (i.e., percentage of time some job is running).
* I/O Utilization (i.e., percentage of time some job is blocked).
* Throughput, expressed in processes completed per hundred time units.
* Average turnaround time.
* Average waiting time.

For example, this is the expected output of input-07 for FCFS scheduling algorithm:
```
The original input was: 3 (0 1 20 1) (0 1 20 1) (10 1 10 1) 
The (sorted) input is:  3 (0 1 20 1) (0 1 20 1) (10 1 10 1) 

The scheduling algorithm used was First Come First Served

Process 0:
    (A,B,C,M) = (0,1,20,1)
    Finishing time: 49
    Turnaround time: 49
    I/O time: 19
    Waiting time: 10

Process 1:
    (A,B,C,M) = (0,1,20,1)
    Finishing time: 50
    Turnaround time: 50
    I/O time: 19
    Waiting time: 11

Process 2:
    (A,B,C,M) = (10,1,10,1)
    Finishing time: 39
    Turnaround time: 29
    I/O time: 9
    Waiting time: 10

Summary Data:
    Finishing time: 50
    CPU Utilization: 1.000000
    I/O Utilization: 0.940000
    Throughput: 6.000000 processes per hundred cycles
    Average turnaround time: 42.666668
    Average waiting time: 10.333333

```

- - - -
## Instructions to run the code 

To run on crackle1 on cims.nyu.edu server, please first go to crackle1, where the folder "Lab2" is uploaded. Once you are at crackle1, please navigate to the Scheduling.java file like the following:
    * Change directory to Lab2, then change directory to src. (cd Lab2/src)
Inside of the "src" folder, Scheduling.java file and Process.java files are located. 

As mentioned above, execute the Scheduling program using the input file name given as the command line argument. The program prints output to the screen as System.out in Java.

**ONE IMPORTANT THING TO NOTE: the input files you are testing the Scheduling.java file against MUST be in the same folder as the Scheduling.java file (namely, the src folder).**


To execute the program using the contents of a text file, please type the name of the java file (Scheduling), the name of any flag (-verbose or -show-random), if any, and the name of the input file. 

Type the below instruction into the Terminal to compile the Scheduling.java program.
When compiling, you have to make sure that you are compiling all the following files: 
* **Scheduling.java** (the Scheduling program that simulates scheduling all the processes to the termination, depending on the different algorithms and the request patterns)
* **Process.java** (instances of ***Process*** are created to store the information about processes we read from the input file. They are utilized by the Scheduling program.)


## Verbose Flag <a name="verbose"></a>
The program accepts an optional "-verbose" flag. If present, it precedes the file name. When "-verbose" is given, the program produce detailed output that's helpful for debugging. 

The program also accepts an optional "-show-random" flag. If present, it precedes the file name. When "-show-random" is given, the program produce detailed output along with the random number chosen each time. This is a more verbose version.

Three possible invocations of the program are:
```
<program-name> <input-filename>
<program-name> --verbose <input-filename>
<program-name> --show-random <input-filename>
```

- - - -
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

- - - -
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
