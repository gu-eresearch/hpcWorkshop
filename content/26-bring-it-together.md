---
title: Bringing it all together
nav: Overview
---

## Some tips and tricks before you get started

You now have everything you need to run jobs, transfer files, use/install software,
and monitor how many resources your jobs are using.

So here are a couple final words to live by:

* Don't run jobs on the login node, though quick tests are generally fine. 
  A "quick test" is generally anything that uses less than 10GB of memory, 4 CPUs, and 15 minutes of time.
  Remember, the login node is to be shared with other users.

* If someone is being inappropriate and using the login node to run all of their stuff, 
  message an administrator to kill their stuff.

* Compress files before transferring to save file transfer times with large datasets.

* Use a VCS system like git to keep track of your code. Though most systems have some form
  of backup/archival system, you shouldn't rely on it for something as key as your research code.
  The best backup system is one you manage yourself.

* Before submitting a run of jobs, submit one as a test first to make sure everything works.

* The less resources you ask for, the faster your jobs will find a slot in which to run.
  Lots of small jobs generally beat a couple big jobs.

* You can generally install software yourself, but if you want a shared installation of some kind,
  it might be a good idea to message an administrator.

* Always use the default compilers if possible. Newer compilers are great, but older stuff generally
  means that your software will still work, even if a newer compiler is loaded.

The following are 2 scripts that will print all prime numbers in a range. Copy one, paste it in your favourite text editor and save the relevant script as an R file extension ( .R) or python file extension ( .py)

R script

```
prime_numbers <- function(n) {
  if (n >= 2) {
    x = seq(2, n)
    prime_nums = c()
    for (i in seq(2, n)) {
      if (any(x == i)) {
        prime_nums = c(prime_nums, i)
        x = c(x[(x %% i) != 0], i)
      }
    }
    return(prime_nums)
  }
  else 
  {
    stop("Input number should be at least 2.")
  }
 }

primes <- prime_numbers(5000)
write.csv(primes, "primes.csv")
```
{: .r}

Python script

```
import pandas as pd

lower = 2
upper = 5000

df = []

for num in range(lower, upper + 1):
   # all prime numbers are greater than 1
   if num > 1:
       for i in range(2, num):
           if (num % i) == 0:
               break
       else:
           df.append(num)
pd.DataFrame(df).to_csv('python_primes.csv')
```
{: .python}


> ## Write a PBS scheduler script for on of the scripts above, chose R or python
> Open your prefered text editor and save the R (.R) or python (.py) scripts.
> Write a PBS scheduler script that will run the above R or python script.
>
> **Important for Windows users** you will need to save your bash scheduler using the software <a href="https://notepad-plus-plus.org/downloads/v7.0/" target="_blank">notepad++</a>. In windows, open up notepad++ and create your scheduler script, then go to "edit" -> "EOL conversion" -> select "UNIX" then save as .sh file. If you don't do this the HPC will terminate your job.
>
> > ## Solution
> > If running the R script, save the following code as a bash script (i.e. .sh)
> > 
> > ```
> > #!/bin/bash
> > #PBS -m abe
> > #PBS -M emailAdress@griffithuni.edu.au
> > #PBS -N SimpleTest 
> > #PBS -q workq
> > #PBS -l select=1:ncpus=1:mem=2gb,walltime=0:05:00
> > 
> > cd $PBS_O_WORKDIR
> > module load R/3.6.1
> > 
> > R CMD BATCH /export/home/s1234567/nameOfRFile.R 
> > ```
> > {: .bash}
> > 
> > If running the python script, save the following code as a bash script (i.e. .sh)
> > 
> > ```
> > #!/bin/bash
> > #PBS -m abe
> > #PBS -M emailAdress@griffithuni.edu.au
> > #PBS -N SimpleTest 
> > #PBS -q workq
> > #PBS -l select=1:ncpus=1:mem=2gb,walltime=0:05:00
> > 
> > cd $PBS_O_WORKDIR
> > module load  anaconda3/2019.07py3
> > 
> > python  /export/home/s1234567/nameOfRFile.py
> > ```
> > {: .bash}
> > 
> {: .solution}
{: .challenge}


> ## Log into the HPC and upload your R or python script and PBS scheduler script, then run it
>
>
>
> > ## Solution
> > 
> > 
> > Upload your scripts. Remember, you need to be logged into your computer, not the HPC
> > 
> > ```
> > s1234567@PC12345 XXXX~$ scp -r directory/on/local/computer s1234567@gc-prd-hpclogin1.rcs.griffith.edu.au:~ 
> > ```
> > {: .bash}
> > 
> > Log into the HPC
> > 
> > ```
> > s1234567@PC12345 XXXX~$ ssh s1234567@gc-prd-hpclogin1.rcs.griffith.edu.au
> > 
> > [s1234567@gc-prd-hpclogin1 ~]$
> > ```
> > {: .bash}
> > 
> > Check that you are in your working directory, then list the files that are there. Make sure that the files from the previous step are there.
> > 
> > ```
> > [s1234567@gc-prd-hpclogin1 ~]$ ls
> > prime_numbers.R   scheduler.sh
> > ```
> > {: .bash}
> > Run the job
> > ```
> > [s1234567@gc-prd-hpclogin1 ~]$ qsub scheduler.sh
> > 49243.gc-prd-hpcadm
> > ```
> > {: .bash}
> > 
> {: .solution}
{: .challenge}
