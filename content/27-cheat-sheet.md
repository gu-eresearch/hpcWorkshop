---
title: Quick start Guide
nav: Quick start Guide
---
## Common UNIX commands

* ls list all visible files and directories
* cd change directory
* pwd print the working directory
* exit 
* more print out a file in the CLI window
* mkdir make a new directory
* rm delete a file, use the -r to delete a directory.


## General process for running a job on Griffith's HPC

## Log onto the HPC

Open your prefered UNIX shell terminal (mac), gitbash (windows).
```
s1234567@PC12345 XXXX~$ ssh s1234567@gc-prd-hpclogin1.rcs.griffith.edu.au
```
{: .bash}

You know your logged in when the CLI address changes.
Your local computer
```
s1234567@PC12345 XXXX~$
```
{: .bash}

HPC address
```
[s1234567@gc-prd-hpclogin1 ~]$
```
{: .bash}

## move files to the HPC

Open your prefered UNIX shell terminal (mac), gitbash (windows) and use the scp command which takes 2 arguments the directory were you would like to move the file to and the directory you would like to move the data from. Remember, if you are moving files from your computer to the HPC, you will need to be logged into your computer.

```
scp directory/on/local/computer/file.txt s1234567@gc-prd-hpclogin1.rcs.griffith.edu.au/~
```
{: .bash}

## Setup a scheduler script

Open up your favourite text editor such as sublime, notepad++ or vscode and write a bash script 

```
#!/bin/bash
#PBS -m abe
#PBS -M emailAdress@griffith/griffithuni.edu.au
#PBS -N SimpleTest 
#PBS -q workq
#PBS -l walltime=300:00:00
#PBS -l select=1
#PBS -l ncpus=1
#PBS -l ngpus=2
#PBS -l mem=32gb

cd $PBS_O_WORKDIR
module load R/3.6.1

R CMD BATCH /export/home/<sNumber>/nameOfRFile.R ## the directory on the HPC used to navigate to and run the script (in this case R)
```
{: .bash}

## Run the job

In your command line interface, make sure your logged into the HPC, and make sure your files (scripts and data) are in your home directory. Then run the following commands.

Run your job
```
[s1234567@gc-prd-hpclogin1 ~]$ qsub example-job.sh
```
{: .bash}

Get stats on your job
```
[s1234567@gc-prd-hpclogin1 ~]$ qstat -x jobID
```
{: .bash}
OR 
```
[s1234567@gc-prd-hpclogin1 ~]$ qstat -u s1234567
```
{: .bash}

Delete your job
```
[s1234567@gc-prd-hpclogin1 ~]$ qdel jobID
```
{: .bash}
OR
```
[s1234567@gc-prd-hpclogin1 ~]$ qdel -u s1234567
```
{: .bash}

## move files from the HPC

Remember, if you are moving files to your computer from the HPC, you will need to be logged into the HPC.

```
s1234567@PC12345 XXXX~$ scp s1234567@gc-prd-hpclogin1.rcs.griffith.edu.au/~ directory/on/local/computer/file.txt
```
{: .bash}


## Some more examples of PBS scripts

## Setup a scheduler script using an anaconda environment

Open up your favourite text editor such as sublime, notepad++ or vscode and write a bash script 

```
#!/bin/bash
#PBS -N condatest
#PBS -m abe
#PBS -M YourEMAIL@griffith.edu.au
#PBS -l select=1:ncpus=1:mem=4gb,walltime=10:00:00
module load anaconda/4.00py2.7
source /sw/anaconda/py27-ver400/etc/profile.d/conda.sh 
##use /sw/anaconda/py3-ver400/etc/profile.d/conda.sh if using “module load anaconda/4.00py3.5”
echo "Starting job: "
cd  $PBS_O_WORKDIR
python <pythonFile.py>
 
>>>An example of the pythonFile.py>>>>>>>>> 
cat pythonFile.py
import tensorflow as tf

## Do python stuff
## Do more python stuff
## Do even more python stuff
>>>>>>>>>>>>>> 
```
{: .bash}