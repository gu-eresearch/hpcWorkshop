---
title: Parallel computing
nav: Parallel computing
---

## What is parallel computing
Parallel computing is a computation that takes a large job and breaks it up into smaller chunks that can be computed at simulatneously. Each individual comptation is then combined together for the final computuation.


## Why use it

## More CPU's != quicker processing time
Below shows Amdahs Law, which is used to show the latency in speedup of computation with each extra CPU used. As you can see it's not linear, its exponential, and it slows down with each extra CPU.
This occurs as there are inefficiencies involved with breaking the dataset into small chunks, sending each of those chunks to each CPU, then after processing is complete, pulling all the individual chunks back together.

![AmdahlsLaw](../files/AmdahlsLaw.png)

Also, not all scripts/jobs can be run in parallel. Maybe only a portion of it can be run in parallel. This is because the dataset is broken up

## When should parallel computing be implemented

Parallel computing is suited to repeditive tasks, that are independent of each other and can be broken up into small chunks. 

To parallelise your job, it needs to:

* be broken down into discrete pieces of work that can be computed simultaneously
* Execute multiple program instructions at any moment in time;
* Be solved in less time with multiple compute resources than with a single compute resource.

There are two major types to think about, data-centric and computation-centric partitioning.


### Examples
Data-centric partitioning - partitioning a dataset into small chunks that can be individually processed
Computation-centric partitioning - Using a for loop to read a large number of csv files into the HPC.
Computation-centric partitioning - Performing cross validation on classification or regression models.

## How do a parallelise and analysis

Some software will require that you setup parallel processing in the scheduler script, and some within the analysis script itself. Python and R are setup in the analysis script.

## Useful resources

## 
https://conf-ers.griffith.edu.au/display/GHCD/Griffith+HPC+User+Guide#GriffithHPCUserGuide-6.2OpenMPandOpenMPSMP-ParallelProgramCompilation.........................................

### R

Basic <a href="https://nceas.github.io/oss-lessons/parallel-computing-in-r/parallel-computing-in-r.html" target="_blank">tutorial</a> to introduce how to parallelise for loops and apply functions.

<a href="https://blog.dominodatalab.com/multicore-data-science-r-python/" target="_blank">Parallel examples</a> for: apply functions, writing large csv's, regression and classification model training using the "caret" package, data munging with the package "dplyr"

### Python 

<a href="https://blog.dominodatalab.com/multicore-data-science-r-python/" target="_blank">Parallel examples</a> for: regression and classification model training using the "scikit-learn" package,

<a href="https://waterprogramming.wordpress.com/2019/04/24/parallelization-of-c-c-and-python-on-clusters/" target="_blank">Parallel examples</a> implementing MPI with python's MPI4Py package


```
import numpy as np

#define an array of numbers
foo = np.array([1,2,3,4,5,6])

#define a function that squares each number
def bar(x):
    return x * x

#map through the array
list(map(bar, foo))
```
{.python}

Is there a dependency of one part of the script on another?

```
import multiprocessing

#define an array of numbers
foo = np.array([1,2,3,4,5,6])

#define a function that squares each number
def bar(x):
    return x * x

#create a pool of processes
with multiprocessing.Pool() as pool:
    result = pool.map(np.square, foo)

#print the results
print(result)
```
{.python}