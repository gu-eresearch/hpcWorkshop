---
title: Containerisation practice
nav: Containerisation practice
---

Lets build an image to run the following python script on the HPC. You will need to pull the latest python image, you also need to install the pandas and numpy packages to run the following script.

Save it as python_primes_np.py

```
import pandas as pd
import numpy as np


lower = 2
upper = 5000

df = []

for num in range(lower, upper + 1):
    if num == np.nan:
        print("One of the values you provided is not a number")
        exit()
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

> ## Pull a singularity image to run python_primes_np.py. You will also need a relevant scheduler script.
>
>
> > ## Solution
> > 
> > I've found the following image on the docker repository https://hub.docker.com/r/amancevice/pandas
> > 
> > First pull the image
> > ```
> > name@name-VirtualBox:~/Documents/singularity/images$ singularity pull --name python_primes docker//:amancevice/pandas
> > ```
> > {: .bash}
> > 
> > Run the image locally
> > ```
> > name@name-VirtualBox:~$ singularity exec python_primes python python_primes_np.py 
> > ```
> > {: .bash}
> > 
> > If it worked, you should have a csv file called python_primes.csv
> > 
> > Now you will need to create a PBS scheduler script to run your singularity image on ther HPC. Save the following scheduler script as scheduler.sh
> > ```
> > #!/bin/bash -l
> > #PBS -m abe
> > #PBS -M yourEmail@griffith.edu.au
> > #PBS -V
> > #PBS -N TestContainer
> > #PBS -q workq
> > #PBS -l select=1:ncpus=1:mem=2gb,walltime=0:05:00
> > cd  $PBS_O_WORKDIR
> > 
> > module load singularity                 # load singularity
> > singularity run python_primes.sif     # add the following bash command so that singularity executes the image during runtime
> > ```
> > 
> {: .solution}
{: .challenge}


> ## Build a singularity image from a singularity definitions file to run python_primes_np.py
>
> > ## Solution
> > The following definition script should be saved as python_np_pd.def
> > ``` 
> > Bootstrap: docker
> > From: ubuntu:18.04
> > 
> > %post
> >     apt-get update 
> >     apt-get -y install python3 
> >     apt-get -y install python3-pip 
> >     pip3 install pandas:1.2.0
> >     pip3 install numpy:1.19.4 
> >     
> >     rm -rf /var/lib/apt/lists/* 
> > 
> > %labels # these are labels that will appear when someone inspects the singularity image
> >     Author eResearch Griffith University
> >     Version v0.0.1
> > 
> > %help
> >     This image is built using ubuntu 18.04 OS. It installs python3.8, pandas1.2.0 and numpy 1.19.4. 
> >     A python script is included that calculates all prime number up to 5000 and writes then in a csv output called python_primes.csv
> >     Use the singularity run command 
> >
> > %files
> >     python_primes_np.py
> > 
> > %runscript
> >     python3 /python_primes_np.py
> > ```
> > {: .bash}
> >
> > Now build a singularity image from the definitions file
> >
> > ```
> > name@name-VirtualBox:~/Documents$ sudo singularity build python_np_pd.sif python_np_pd.def
> > ```
> > {: .bash}
> > Now we have a singularity image called python_np_pd.sif
> > 
> {: .solution}
{: .challenge}

> ## Use docker to pull an image and then convert it to a singularity image that will run python_primes_np.py
> This exercise is only for people who know how to use docker. It does not do anything that singularity cannot alread do.
> Have a look on the <a href="https://hub.docker.com/" target="_blank">docker repository</a> for a suitable image
>
> > ## Solution
> > Lets find a docker image with python, pandas and numpy and pull it using docker. I looked on docker and found this image: https://hub.docker.com/r/amancevice/pandas. So I will pull it, -slim means that I will pull a smaller version of python. This image should contain numpy. This is because pandas is reliant on numpy and will install it when pandas is installed.
> >
> > ```
> > name@name-VirtualBox:~$ docker pull amancevice/pandas:1.2.0-slim
> > 1.2.0-slim: Pulling from amancevice/pandas
> > 6ec7b7d162b2: Pull complete 
> > a28a49c1de56: Pull complete
> > .......
> > 
> > name@name-VirtualBox:~$ docker image ls
> > REPOSITORY                               TAG                   IMAGE ID       CREATED         SIZE
> > quay.io/singularity/docker2singularity   latest                e07682c30060   6 days ago      407MB
> > amancevice/pandas                        1.2.0-slim            5d6c6e32c6ab   3 weeks ago     247MB
> > continuumio/miniconda3                   latest                52daacd3dd5d   6 weeks ago     437MB
> > ```
> > {: .bash}
> > Then run the docker to singularity image.
> > 
> > ```
> > docker run -v /var/run/docker.sock:/var/run/docker.sock -v ~/Documents/HPC:/output quay.io/singularity/docker2singularity --name pandasprimes amancevice/pandas:1.2.0-slim
> > Image Format: squashfs
> > Docker Image: amancevice/pandas:1.2.0-slim
> > Container Name: pandasprimes
> > 
> > Inspected Size: 247 MB
> > 
> > (1/10) Creating a build sandbox...
> > .......
> >
> > ```
> > {: .bash}
> > 
> > You now have a singularity image named pandasprimes.sif
> > 
> {: .solution}
{: .challenge}


> ## Make a PBS scheduler script to run the your image
>
> > ## Solution
> > 
> > You will need to create a PBS scheduler script to run your singularity image. Save the following scheduler script as scheduler.sh
> > ```
> > #!/bin/bash -l
> > #PBS -m abe
> > #PBS -M yourEmail@griffith.edu.au
> > #PBS -V
> > #PBS -N TestContainer
> > #PBS -q workq
> > #PBS -l select=1:ncpus=1:mem=2gb,walltime=0:05:00
> > cd  $PBS_O_WORKDIR
> > 
> > module load singularity                 # load singularity
> > singularity run pythonContainer.sif     # add the following bash command so that singularity executes the image during runtime
> > ```
> > {: .bash}
> > 
> {: .solution}
{: .challenge}


> ## upload the singularity image and pbs scheduler script to the HPC and run it
>
> > ## Solution
> > 
> > Copy the 
> > 
> > ```
> > s1234567@PC12345 XXXX~$ scp -r directory/on/local/computer/ s1234567@gc-prd-hpclogin1.rcs.griffith.edu.au:~ 
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
> > prime_numbers.py   scheduler.sh
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