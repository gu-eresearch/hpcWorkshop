---
title: Using a HPC
nav: Using a HPC
---

With all of this in mind, we will learn how to connect to a HPC cluster (if you haven't done so already!). 
We will look at the theory and what is required to connect to Griffith Universites HPC.
Although it's unlikely that every system will be exactly the same, 
it's a very good example of what you can expect from a HPC.


There are several steps we will need to follow to run analysis on a HPC. 
* write a batch script (used by the HPC scheduler). This tells the HPC how many resources you need (i.e. number of CPU's and amount of time it will take to run), where your scripts and data are located within the HPC and the software you would like to use. 
* copy your scripts and data accross to the HPC.
* check that the packages/modules are loaded onto the HPC. 

In the following we will go through these steps in detail.
