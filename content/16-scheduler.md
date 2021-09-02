---
title: Scheduling jobs
nav: Scheduling jobs
---

Our data and script for processing are loaded onto the HPC and are located in our home folder on the login node (Remeber, don't run jobs on this node). Next we need to tell the HPC to run our scripts. We could run them here, but we won't, because we don't want to stop other users from accessing the HPC.
On the HPC, a piece of software known as a scheduler manages which jobs run where and when.

The scheduler used on Griffith's HPC is the PBS batch queuing system.
PBS is not used everywhere, however
running jobs is similar regardless of the software being used and although the exact syntax might change, the concepts will remain the same.

## Running a batch job

The most basic use of the scheduler is to run a command non-interactively.
This is also referred to as batch job submission.
In this case, a job is just a shell script.
The following is a demo shell script (file extension .sh) used by the PBS scheduler to run your script on the HPC (in this case an R script).

**Important for Windows users** you will need to save your bash scheduler using the software <a href="https://notepad-plus-plus.org/downloads/v7.0/" target="_blank">notepad++</a>. In windows, open up notepad++ and create your scheduler script, then go to "edit" -> "EOL conversion" -> select "UNIX" then save as .sh file. If you don't do this the HPC will terminate your job. You can also use dos2unix to change the format from dos to unix and vice versa.

```
#!/bin/bash
#PBS -m abe
#PBS -M emailAdress@griffith/griffithuni.edu.au
#PBS -N SimpleTest 
#PBS -l select=1:ncpus=1:mem=12gb,walltime=0:01:00

cd $PBS_O_WORKDIR
module load R/3.6.1

EXECUTION PROGRAM /export/home/<sNumber>/nameOfRFile.R # the directory on the HPC used to navigate to and run the script (in this case R)
results.Rout # output from the above r script
```
{: .bash}

## Breakdown of the above shell script
* **#PBS** represent the commands directed at the PBS scheduler. While the rest are the tasks/commands to be carried out.
* **#PBS -m** The -m flag is used for specifying email notifications. The options **a b e** send an email when: the job is aborted by PBS, the job begins execution, the job ends (i.e. finishes).
* **#PBS -M** The -M flag is used to specify the email address to send confirmation that the job was sucessfully loaded into the HPC. This is far better than constantly checking the status of your job.
* **#PBS -N** the -N flag is used to name the job submitted
* **#PBS -l** The -l flag takes multiple arguments used to request HPC resources for you job.
   * **select** The number of chunks you would like to break the job up into. Generally keep it as 1 chunk unless you are submitting a job that uses alot of resources, this way the job will be submitted as multiple smaller chunks that will be processed quicker.
   * **ncpus** Number for cpu's to allocate; 
   * **ngpus** Number for gpu's to allocate; 
   * **mem** Amount of memory to allocate, by default it is in bytes unless you specify gb or mb; 
   * **walltime** maximum amount of time allocated to the job in hours:minutes:seconds. Requesting to much walltime can prevent the job from running as the scheduler will keep the job in the que waiting for a large enough opening. While to little walltime will result in the job getting terminated before its finished processing. Later on, we will look into estimating the required wall time.
   * **ngpus** if not using gpu's.
This is followed by several bash commands (don't start with #PBS)
* **cd $PBS_O_WORKDIR** PBS_O_WORKDIR will be the directory where the pbs script resides. If you don't add `cd PBS_O_WORKDIR` to your scheduler script, all output files will be put in the home directory. It is OK for a few runs but after a while it will clutter the home directory with lots of files. You can also add a specific directory if you would like to write the output into a certain directory.
* **module load** list the modules you require to run your analysis. We will look at how to identify with modules are on the HPC
* **EXECUTION PROGRAM /export/home/<sNumber>/nameOfRFile.R** First tell the HPC which program to run for your analysis, and then the path to our folder on the HPC and the name of the script to run. If using running R use "R CMD BATCH".

> ## What's wrong with the following scheduler script
> ```
> #!/bin/bash
> #PBS -N simple_failing_test
> 
> echo 'This script is running on:'
> hostname
> sleep 120
> ```
> {: .bash}
> This is the error that will be thrown
> ```
> qsub: Job has no walltime requested
> ```
> {: .bash}
> > ## Solution
> > It's missing the walltime argument which is required. Below is the script with walltime added.
> > ```
> > #!/bin/bash
> > #PBS -N simple_failing_test
> > #PBS -l walltime=00:03:00
> > 
> > echo 'This script is running on:'
> > hostname
> > sleep 120
> > ```
> > {: .bash}
> > 
> {: .solution}
{: .challenge}

> ## Customizing a job
>
> Write the relevant section of a PBS scheduler script that will run on the HPC using 2 cpu's, 4 gigabytes of memory, and 5 minutes of walltime.
>
> > ## Solution
> > 
> > ```
> > #PBS -l walltime=00:05:00: ncpus=2: mem=4gb
> > ```
> > {: .bash}
> > 
> {: .solution}
{: .challenge}

> ## Write output files to a specific folder
>
> Write the relevant section of a PBS scheduler script that will write output files to the folder '/export/home/s1234567/HPC_Tutorial'
>
> > ## Solution
> > 
> > ```
> > cd /export/home/s1234567/HPC_Tutorial
> > ```
> > {: .bash}
> > 
> {: .solution}
{: .challenge}

## Running jobs with large resource requirements
If you plan to run a job with high resource requirements (i.e. large memory or large wall time) then you should place it in the routeq queue. If you don't do this then your job may take weeks to months before the HPC even starts to execute you analysis.

Add the following in your scheduler script to run in the high resource queue.
```
#PBS -q routeq
```
{: .bash}