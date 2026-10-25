# CS 143A Project 1

## 1. Introduction

In this programming assignment, you will be provided an OS simulator for which you must implement various kernel syscalls and cpu interrupts. By the end of the project, you will implement a CPU scheduling algorithm and become familiar with the simulator. The simulator will be expanded in future projects so be sure to spend some time to understand how it works.

**Programming language:** The assignment will be implemented in Python.

**Cooperation and third party code:** This is a group programming assignment, and your group should implement the code on your own. Your group may not share final solutions with anyone outside of your group, and your group must write up your own work, expressing it in your own words/notation. Third party code including libraries are not allowed (built in python libraries are fine).

## 2. Environment setup

The simulator runs using standard Python without any installed libraries. Your code will be graded with **Python 3.10.12**, so we recommend using [conda](https://docs.conda.io/en/latest/) to create an environment with exactly that version. If you do not have conda yet, install [Miniconda](https://docs.conda.io/en/latest/miniconda.html).

From the project folder (the one containing `requirements.txt`), create and activate the environment:

```
conda create --name cs143a --file requirements.txt
conda activate cs143a
python --version   # should print Python 3.10.12
```

Activate the environment (`conda activate cs143a`) every time you open a new terminal to work on the project. The same environment can be reused for later projects.

You should avoid writing any code that is OS dependent as no test will require this and it could cause problems when grading.

## 3. Getting familiar with the code base

### 3.1 Downloading the code

You can download the code [here](https://drive.google.com/file/d/1eTSPoCCwdm073gVrBr1W9E8Cqs0sYUDc/view?usp=sharing) (be sure to download it with your uci email). Once you have downloaded the code, unzip it anywhere you would like. Before writing any code, go to the top of `kernel.py` and fill in your group's information.

### 3.2 Code overview

The simulator consists of two files. `simulator.py` and `kernel.py`. `simulator.py` simulates the CPU based on the provided simulation parameters. You should make no modifications to `simulator.py` as these changes will NOT be included in your submission. If needed, you can make a copy of the simulator and modify it as you wish for testing purposes, but make sure your code works with the unmodified simulator before submitting.

Simulations can be found in the `simulations` folder. Each file is a different simulation which will be tested. Every simulation in `simulations` has a corresponding correct log file in the `correct_output` folder. You can compare your output with these files to test your code. If your output does not match these files exactly (including white space), your code will fail this test. (Note: You may find commands like `diff` report differences when there are no visible differences. This could be due to different operating systems having different byte patterns for new lines in files. As long as the text matches including whitespaces, it will pass the autograder). Not all tested simulations are provided, however the provided test cases cover the majority of features and scenarios that the hidden test cases will. Additionally, you can view your score on the hidden test cases after submitting and resubmit as many times as you need.

Upon running the simulator, it will import the `Kernel` class in `kernel.py`. This is where your code will be. In this file, you will see a number of methods. These methods represent the various syscalls and interrupts a kernel must handle. For the time being there are only a few methods, but as more projects are released, more methods will be added. Be sure not to modify any of these functions names or arguments otherwise the simulator will not function properly. These functions are automatically called by the simulator as various events occur. Please read through all of `kernel.py` to fully understand what each method does.

Most of these functions return a PID. The returned PID represents the process that the kernel will give CPU time next. At the beginning, the process with PID 0 (or the idle process) is running. This is the default process to run when there is nothing else to run on the system. As the various methods are called, your implementation of the kernel should return the PID of the process you believe should be running. This depends on which scheduling algorithm has been selected for this simulation. This is provided automatically by the simulator when calling the `__init__` method. For this first project, there is only 1 scheduling algorithm (FCFS). The scheduling algorithm is defined in the simulation file in `simulations`. It is your responsibility to keep track of what process is running at any given time. This is important because as syscalls are issued, they are issued by the currently running process and should be handled as such.

As the simulator runs, it will automatically generate a log file. This file is the same as those you will find in the `correct_output` folder. The log file will include the various events that each process triggers as well as any context switches initiated by the kernel. These will be time stamped based on their simulated time. **IMPORTANT:** The timestamps have no correlation with real time and are entirely simulated. Your code will not need to write anything to this log file as it is automatically generated by the simulator. Additionally, this log file is what will be tested for correctness. Any print statements in your code will be ignored by the autograder.

For debugging purposes, a logger is provided by the `__init__` method. This logger has one method (`.log`) which has one `str` argument. Any `str` given will be printed to the log file with a timestamp. These additional messages will be excluded when grading so feel free to leave them in when submitting.

### 3.3 Running the simulator

To run the simulator, simply run the command:

```
python3 simulator.py <simulation.json> <output_log.txt>
```

An example is:

```
python3 simulator.py simulations/FCFS1.json output.txt
```

Alternatively if you would like to run it without your custom log messages included you can run:

```
python3 simulator.py <simulation.json> <output_log.txt> --no-student-logs
```

If this is your first time running the simulator and you have not modified `kernel.py` yet you may see this output:

```
SimulationError: Process 0 (idle process) has been running for 1 second straight.
            This will not happen in tested simulations and is likely a bug in the kernel.
```

This is because the provided framework always runs the idle process or PID 0. The simulator automatically detected that PID 0 has been running for over a second. Since PID 0 essentially does nothing, the simulation will run forever. To prevent this, it will end the simulation early. With correct implementations, none of the test cases will have PID 0 run for this long.

## 4. What to implement

The primary goal of this first project is to learn the framework which will be expanded upon in future projects. To get started, you will implement one scheduling algorithm: First Come First Served (FCFS). Secondly, you will calculate some statistics about the scheduler such as turnaround time and wait time.

### 4.1 FCFS

Implement a FCFS scheduler. Processes are run in arrival order.

### 4.2 Scheduler Statistics

Calculate the turn around time and waiting time for each process in each of the simulations. To do this you can use the correct output's timestamps for each event. Put your answers in `stats.json` keeping the same format as it originally had. Format your answers as integer numbers in milliseconds (ms). If your answer is not an integer, round it down to the nearest whole number. Eg: `"540ms"`. Finally, calculate the average turn around time and waiting time for all processes.

## 5. Submitting

When submitting your code you should ONLY submit `kernel.py` and `stats.json`. Do not zip the files, just submit them both. They must have these exact names. Upload these files to Gradescope. If after submitting, if you are unsatisfied with your score, you may submit again as many times as you like. We will only grade the most recent submission. Be sure to submit as a group in Gradescope.
