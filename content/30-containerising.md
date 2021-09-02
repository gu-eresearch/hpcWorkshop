---
title: Containerisation for HPC
nav: Containerisation
---

## What is containerisation, and why is it amazing?
A container is a standard unit of software that packages up entire scientific workflows. This includes the operating system (ubunut, centos ect.) software (python, R ect.), dependencies (pandas, numpy, ggplot, dplyr ect.), your code and even the data so that your analysis runs quickly and reliably on any computer system. Singularity is a container software specifically for use in a HPC environments. Containerising your scientific workflow has several major benefits: portability, scalability and reproducibility. 

Ok, so why is that relevant to scientific computing in a HPC cluster? Say you have a workflow on your computer and you want to move it to the HPC. If a software, or the correct versions of software are not avaliable, you can ask the administrator to load it into the HPC cluster. However, sometimes packages cannot be installed, or are very nasty to install on a HPC cluster. A better solution is to build a container that has everything needed to run your analysis. This has the added advantage of being portable and reproducable on different HPC's across institutions. For example you or a colleague may want to run your workflow on a larger HPC, all they need is your singularity container and they can run the same analysis and get the same output. 
Containerising your workflow also provides greater transparency and scientific reproducibility. We have all looked at the methods section in a research paper and though ok, I understand in theory what you have done, but how did you actually do it. The researcher than provide you with their containerised workflow and you will be able to look at the code, run the code and see exactly how the analysis was performed.

Singularity is a containerisation software specific for HPC environments, for a detailed analysis of the benefits singularity has for scientific computing, have a look at <a href="https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0177459" target="_blank">this paper.</a>


## Containerising my scientific workflow with singularity
To containerise your workflow you will need to build or pull an image that contains the software and packages you require to run your scientific workflow. You will then need to update your PBS scheduler script to run the container on a HPC. Before going further, lets define containers and images. An image packs up the software, the code and data into a single environment that will run, and a container is a running instance of the image.

There are two different ways that you can containerise your workflow using singularity images for HPC's. The first is using a <a href="https://www.linux.com/what-is-linux/" target="_blank">linux operating system</a> (OS) to build, pull, or interact with a singularity image file (a file ending in .sif). We will use a linux OS in this tutorial.
The second is using <a href="https://docs.docker.com/get-started/overview/" target="_blank">docker</a>, a containerisation software used extensively in information technology. Docker can be used to build, or pull a docker image and then convert it to a singularity image. The beauty of docker it that it can be used on windows and mac OS's. Examples of using docker have been included for later reference, although we will not get into docker in the tutorial.

## Singularity images avaliable on Griffith's HPC

Before building or pulling images have a look on the Griffith HPC as there are a number of singularity images already avaliable. You will need to log into the HPC and navigate to the directory where singularity images are stored. The fiels that end in .sif are the latest singularity images, files that end in .simg are the older style of singularity images, and .img are docker images, they need to get converted to .sif.
```
[s1234567@gc-prd-hpclogin1 ~]$ cd /sw/Containers/singularity/images
[s1234567@gc-prd-hpclogin1 images]$ ls
centos7.img           installNotes.txt                 singularity-firefox_latest.sif  Ubuntu2.img                   xbeach_latest.sif
chrome_latest.sif     MitoZ.simg                       swan_latest.sif                 ubuntu.img
delft3d4_latest.sif   nanodock.sif                     test-image_latest.sif           ubuntu-writable-yade.simg
delft3dfm_latest.sif  openEMS-ubuntu-16.04.img         ubuntu-16.04.4-10GB.img         ubuntu-yade.simg
delft3d_latest.sif    openfoam7-paraview56_latest.sif  ubuntu2-conda-tensorflow.img    wflow_latest.sif
```
{: .bash}


# Using a Linux distribution to build, pull or interact with a singularity image
Using the following method, you can use singularity directly to build, pull and interact/update singularity images.
First you will need to setup the linux distribution on your computer. You will need to install a linux distribution on a bootable usb or install a virtual machine to run a linux distribution. There are pro's and con's to each methods, you should do some research to decide which suits your needs. 
If you decide to use a bootable usb, instructions are found for <a href="https://ubuntu.com/tutorials/create-a-usb-stick-on-macos#1-overview" target="_blank">windows</a> and for <a href="https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#1-overview" target="_blank">mac</a>
If you would prefer to install a virtual machine, <a href="https://www.virtualbox.org/" target="_blank">virtual box</a> is a good option.

You can <a href="https://sylabs.io/guides/3.5/user-guide/quick_start.html" target="_blank">install singularity 3.5x</a> or higher in your linux distribution.
The official installation guide for singularity can be accessed <a href="https://singularity.lbl.gov/quickstart#installation" target="_blank">here.</a>


## Pull (Download) a prebuilt image

Before building a singularity image from scratch, have a look on the following image repositories and see if there is one already avaliable. You will need to use the tag (which is actually the repo's URI) to pull images from these repositories.

Repositories that contain useful images
* <a href="https://quay.io" target="_blank">Quay</a> has images for all applications. Use the tag "Quai://"
* <a href="https://hub.docker.com/" target="_blank">Docker</a> has images for all applications. Use the tag "docker://"
* <a href="https://ngc.nvidia.com" target="_blank">NDIVIA</a> has images for GPU purposes. Use the tag "NDIVIA://"
* <a href="https://cloud.sylabs.io/library" target="_blank">Singularity</a> images for all applications. Use the tag "library://"

Images for specific software
* Python <a href="https://hub.docker.com/_/python" target="_blank"> official docker repo</a>
* Conda <a href="https://hub.docker.com/r/continuumio/miniconda3" target="_blank"> official docker repo</a>, use the tag "docker://continuumio/miniconda3:version" 
* <a href="https://biocontainers.pro/registry" target="_blank">Biocontainers</a> is a registry specifically for bioinformatics images

We will use the singularity pull command to pull a pre-built image from the official Singularity <a href="https://cloud.sylabs.io/library" target="_blank">repository</a>. The pull command downloads the image as a singularity image file (.sif), regardless of the repository it was pulled from.
When pulling an image you need to specify the repository your pulling from followed by the image's name. 

Lets pull the chuanwen/cowsay image from the docker repository and run it.
Remember, you can pull docker images with the singularity pull command using the "docker://" tag. Docker images will be converted to a singularity image when using the singularity pull command. 
``` 
name@name-VirtualBox:~$ singularity pull docker://chuanwen/cowsay
INFO:       Converting OCI blobs to SIF format
........
........
INFO:       Build complete:   cowsay_latest.sif
```
{: .bash}

Nice we've pulled a docker image. Now before we run it lets look at the help. Use the singularity run-help command.
```
name@name-VirtualBox:~$ singularity run-help cowsay_latest.sif
No help sections were defined for this image
```
{: .bash}

Well that was not informative. Let try running the image using the singularity run command. 

```
name@name-VirtualBox:~$ singularity run cowsay_latest.sif
 _____________________________
/ The ends justify the means. \
|                             |
\ -- after Matthew Prior      /
 -----------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

name@name-VirtualBox:~$ 
```
{: .bash}

Nice, it ran. So thats what cows say.

In the next example we will pull an image from the singularity repository using the tag "library://". We will pull the following image "dppereyra/default/python:3.6.8-debian". This image contains python version 3.6.8 with a debian OS (debian is a linux distribution).

The -n or --name flag let you set a name to later reference the image with.

By default the image will be saved in the current working directory. If you are not sure of your current working directory, you can use the pwd command to print it out. In the below example it will be "/Documents/singularity/images".

```
name@name-VirtualBox:~/Documents/singularity/images$ singularity pull --name python_deb library://dppereyra/default/python:3.6.8-debian 
```
{: .bash}


If successful you should see something similar to this
```
INFO:    Downloading library image
 65.40 MiB / 65.40 MiB [===========================] 100.00% 451.54 KiB/s 2m28s
WARNING: Container might not be trusted; run 'singularity verify python_3.6.8-debian.sif' to show who signed it
INFO:    Download complete: python_deb
```
{: .bash}

As a security measure when pulling from the singularity repository, you can verify the cryptographic signature of the image. 
```
name@name-VirtualBox:~$ singularity verify python_deb
Container is signed by 1 key(s):

Verifying partition: FS:
82B6ACA1EB78FB6D32750F01BB9973A20ECD72F1
[REMOTE]  DPPereyra <dppereyra@gmail.com>
[OK]      Data integrity verified

INFO:    Container verified: python_deb
```
{: .bash}

Congratulations, now you have pulled a singularity image. 
Run this image locally on your computer to test it works. Lets check the version of python installed in the image python_deb.

```
name@name-VirtualBox:~$ singularity exec python_deb python --version
Python 3.8.0
```
{: .bash}

What we did was execute the singularity image "python_deb" using the exec command. We added an extra command onto the end "python --version", which printed out the version of python in python_deb, you can run this same command on you computers bash terminal and it will print out the version of python installed on your computer.

Now save the following script as python_primes.py and run it using the python_deb image.

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

```
name@name-VirtualBox:~$ singularity exec python_deb python ~/Documents/pythonScript.py
Traceback (most recent call last):
  File "/home/brett/Documents/pythonScript.py", line 1, in <module>
    import pandas as pd
ModuleNotFoundError: No module named 'pandas'
```
{: .bash}

> ## What is wrong??
>
> > ## Solution
> > 
> > The image we pull contains python, but does not contain the pandas package required to run our python_primes.py script.
> > 
> {: .solution}
{: .challenge}

Lets explore this a bit further.
Singularity has a command "shell" that lets you enter the bash terminal within the running singularity container. Remember, the singularity image python_deb has a debian OS (linux distribution), which contains a bash terminal. 
So we can enter python using the bash terminal and see if pandas is installed.

```
name@name-VirtualBox:~$ singularity shell python_deb
Singularity> 
```
{: .bash}

Now were in the bash terminal within the running container, we can open python just as we can on any OS. So lets open python and see if pandas is installed by using the python "import" command.

```
Singularity> python
Python 3.6.8 (default, Jun  3 2019, 08:42:23) 
[GCC 6.3.0 20170516] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import pandas
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'pandas'
>>>
```
{: .bash}

Ok, how do we fix this. 

Remember one of the amazing benefits of using singularity images is that they are reproducable? Well we need to include pandas in the image in a way that is reproducable. We can do this in 2 ways. 1. pull an image that includes pandas in the image. 2. Build an image from scratch using a singularity definitions file that specifies everything that we would like in our image.
Next we will look at building an image from scratch using a definitions file. However, there is an example of pulling an image that contains pandas in the next lesson "Containerisation practice".

## Build a singularity image from scratch with a definition file
You can build a singularity image from scratch with a definition file. A definition file defines how singularity should build the image, and then you use the singularity command line to build the image.

Below is a basic singularity definition file. Copy the following script and save it as pythonScript.def

```
Bootstrap: docker
From: ubuntu:18.04 

%post
    apt-get update      
    apt-get -y install python3 
    apt-get -y install python3-pip 
    pip3 install pandas 
    
    rm -rf /var/lib/apt/lists/* 

%labels
    Author eResearch Griffith University
    Version v0.0.1

%help
    This image is built using an ubuntu OS. It has python3 and the pandas package installed. 
    You will need to use the singularity exec command to analysis your data

%files

%runscript

```
{: .bash}

Lets go through each line of code in the definition file


* Bootstrap: docker # Bootstrap indicates the repository to pull from, in this case docker
* From: ubuntu:18.04 # From indicates the image we wish to pull. This example is the official ubuntu image, version 18.04

* %post
> apt-get update      # apt-get is the package manager for ubuntu used to install packages, in the following case python3
> apt-get -y install python3 # install python3. We have not provided a version, so it will pull the latest version. A version number can be added using the python3:version convention
> apt-get -y install python3-pip # install python package manager (pip)
> pip3 install pandas # install the required python packages, in this case the latest pandas version will be installed
>
> rm -rf /var/lib/apt/lists/* # Reduce the size of the image by deleting the package lists we downloaded, which are useless now.
> 
* %labels # these are labels that will appear when someone inspects the singularity image
> Author eResearch Griffith University
> Version v0.0.1

* %help # this is the help that will appear when someone asks for help from the singularity image
> This image is built using an ubuntu OS. It has python3 and the pandas package installed. 
> You will need to use the singularity exec command to analysis your data

* %files # include any files, such as scripts and data. Their paths should be included as found on the HPC if not in the same directory as the image that will be build. 

* %runscript # include any bash commands used to run scripts ect

Now we can build a singularity image from the above definition file. For this we use the build command, which takes 2 arguments, the name that the singularity image will be called and the name of the definition file used toi build the image.

```
name@name-VirtualBox:~/Documents$ sudo singularity build pythonContainer.sif pythonScript.def
```
{: .bash}

Now we have a singularity image. Use the singularity "inspect" command to see the attached metadata of the singularity image. Notice the Author, build date, and build version are the ones we supplied in the definitions file.

```
name@name-VirtualBox:~/Documents$  singularity inspect pythonContainer.sif
WARNING: No SIF metadata partition, searching in container...
Author: eResearch Griffith University
Version: v0.0.1
org.label-schema.build-date: Friday_15_January_2021_14:33:42_AEST
org.label-schema.schema-version: 1.0
org.label-schema.usage: /.singularity.d/runscript.help
org.label-schema.usage.singularity.deffile.bootstrap: docker
org.label-schema.usage.singularity.deffile.from: ubuntu:18.04
org.label-schema.usage.singularity.runscript.help: /.singularity.d/runscript.help
org.label-schema.usage.singularity.version: 3.5.2
```
{: .bash}

All singularity images should contain an understandable help section. To see the help of a singularity image run the singularity "run-help" command, notice that we supplied it in the definitions file.

```
name@name-VirtualBox:~/Documents$ singularity run-help pythonContainer.sif
    This image is built using an ubuntu OS. It has python3 and the pandas package installed. 
    It has a python script included that calculates all prime number up to 5000 and writes then in a csv output called python_primes.csv
    You will need to use the singularity run command 
```
{: .bash}

Now lets execute our analysis using the singularity image we just created. We will use the exec command which takes 2 arguments, the singularity image and the bash command to execute the analysis script. Note that we will run it on our own computer as we have not copied it to the HPC.

```
name@name-VirtualBox:~/Documents$ singularity exec pythonContainer.sif python3 python_primes.py

```
{: .bash}

Now ls and you will see a new file python_primes.csv. Success, this is the output from the pythonScript.py 

In the above example we ran a single bash command "python3 pythonScript.py" as part of the exec command when executing the pythonScript.sif. If we have multiple bash commands we may want to include include them in the def file so that we don't have to type them all out when we run the singularity exec command. In the def file we will include %files and %runscript headings as in the example below.

```
Bootstrap: docker
From: ubuntu:18.04

%post
    apt-get update      
    apt-get -y install python3 
    apt-get -y install python3-pip 
    pip3 install pandas 
    
    rm -rf /var/lib/apt/lists/* 

%labels # these are labels that will appear when someone inspects the singularity image
    Author eResearch Griffith University
    Version v0.0.1

%help 
    This image is built using an ubuntu OS. It installs python3.8 and the pandas1.4. 
    A python script is included that calculates all prime number up to 5000 and writes then in a csv output called python_primes.csv
    Use the singularity run command 

%files
    python_primes.py # add the scripts that need to get processed, this script can be found in the "Bringing it together lesson"

%runscript
    python3 /python_primes.py  # add the bash commands that need to run during runtime to process the scripts in %files
```
{: .bash}

> ## Build this definition file.
>
> > ## Solution
> > 
> > ```
> > name@name-VirtualBox:~/Documents$ sudo singularity build pythonContainerRun.sif pythonScript.def
> > ```
> > {: .bash}
> > 
> {: .solution}
{: .challenge}

Now we can use the singularity command "run" instead of "exec". With therun command we don't include any bash commands as they are already defined in the image and will automatically be executed. 

```
name@name-VirtualBox:~/Documents$ singularity run pythonContainerRun.sif
```
{: .bash}

For a comprehensive guide on def file setup go to the <a href="https://sylabs.io/guides/3.0/user-guide/definition_files.html" target="_blank">offical documentation.</a>

## Sandbox test environment for building definition files
You may not always know exactly what you want to put in your definitions file. In this case it can be a good idea to first build a "sandbox" environment (i.e. throwaway environment for testing) as a way to interactively test each part of your definitions file. When you know that it will work in sandbox you can add the code to the production ready definitions file. We will walk through an example to demonstrate this sandbox idea.

Type the following into your bash terminal

First create a new folder can cd into it. Then use the singularity "build" command with the --sandbox flag, name of the sandbox environment and base image tag to pull.
```
name@name-VirtualBox:~$ cd ~/Documents/mySingularityProject
name@name-VirtualBox:~$ sudo singularity build --sandbox ubuntu_py docker://ubuntu

```
{: .bash}
If successful, you will see the following. This isndicates that the latest ubuntu image has been successfully pull form the docker repository.
```
INFO:    Starting build...
Getting image source signatures
Copying blob da7391352a9b done
Copying blob 14428a6d4bcd done
Copying blob 2c2d948710f2 done
Copying config aa23411143 done
Writing manifest to image destination
Storing signatures
2020/12/21 10:23:49  info unpack layer: sha256:da7391352a9bb76b292a568c066aa4c3cbae8d494e6a3c68e3c596d34f7c75f8
2020/12/21 10:23:51  info unpack layer: sha256:14428a6d4bcdba49a64127900a0691fb00a3f329aced25eb77e3b65646638f8d
2020/12/21 10:23:51  info unpack layer: sha256:2c2d948710f21ad82dce71743b1654b45acb5c059cf5c19da491582cef6f2601
INFO:    Creating sandbox directory...
INFO:    Build complete: ubuntu_py
```
{: .bash}


Next we need to install python and all required packages into our sandbox. We will use singularity shell command with the --writable flag. If we don't use the writable flag then singularity will not save the changes when we exit the shell.
```
name@name-VirtualBox:~$ sudo singularity shell --writable ubuntu_py
```
{: .bash}
Use apt-get to install modules (i.e. python, R ect.). To install python packages use pip. 
```
Singularity > apt-get -y update
Singularity > apt-get install python3
running python post-rtupdate hooks for python3.8...
Processing triggers for libc-bin (2.31-0ubuntu9.1) ...
Singularity > apt-get -y install python3-pip

Singularity > pip3 install pandas


```
{: .bash}

Now lets see if we can run our python script in the sandbox container. 


### Final thoughts about building from a defitition file
* Document your definitions file so that others know how to use it. This can be achieved using the "%help" or "%apphelp" syntax within the definitions file.
* Use sandbox for testing, not production. Using an image built in sandbox can lead to reduced reproducibility.


## Build or pull a docker image with docker and convert to singularity
If you don't have a linux distribution with singularity, and you can use docker, then you can build or pull a docker container and convert it to a singularity container. This is of advantage to people who already know docker, and works on a mac and windows. You will need to have docker installed: click for <a href="https://docs.docker.com/docker-for-mac/install/" target="_blank">mac</a> and <a href="https://docs.docker.com/docker-for-windows/install/" target="_blank">windows</a>

Now you can build a docker container using a "dockerFile", docker's version of singularity definitions file, then convert to singularity and move it to the HPC. 
Once you have built or pulled a docker container, you can run the it locally, but will not be able to run the converted singularity container locally, unless you are using a linux distribution with singularity installed.


Let get started. Pull docker image, lets pull the latest miniconda image from docker.
```
s1234567@PC12345 XXXX~$ docker pull continuumio/miniconda3
```
{: .bash}

Check the docker images on your local computer using the image ls command. Note that this image is called with "continuumio/miniconda3".
```
s1234567@PC12345 XXXX~$ docker image ls
REPOSITORY                               TAG                   IMAGE ID       CREATED         SIZE
continuumio/miniconda3                   latest                52daacd3dd5d   5 weeks ago     437MB
```
{: .bash}

Then run the docker to singularity image.
```
s1234567@PC12345 XXXX~$ docker run -v /var/run/docker.sock:/var/run/docker.sock -v ~/directory/to/save/sif/to:/output quay.io/singularity/docker2singularity --name miniconda3 continuumio/miniconda3
```
{: .bash}
There are several key variable we need to provide when we run docker2singularity:
* "-v /var/run/docker.sock:/var/run/docker.sock"
* "-v ~/directory/to/save/sif/to:/output" The directory on your local computer to save the singularity container, make sure to add ":/output" to the end to the directory path.
* "quay.io/singularity/docker2singularity" The docker2singularity image, in this instance it will automatically pull or update if needed.
* "continuumio/miniconda3" The image you are converting, in this case its continuumio/miniconda3.
* "--name miniconda3" The --name flag allows you to give a custom name to the container, in this case miniconda3.sif.

Now that we have the continuumio/miniconda3 image we can modify it to better suit our specific needs. This can be done by running the image and entering into it's bash terminal. Yes inside the image is a bash terminal, this is because we have an entire operating system that includes bash.
 
# Running your singularity image as a container on Griffith's HPC

You will need to create a PBS scheduler script to run your singularity image. Save the following scheduler script as scheduler.sh
```
#!/bin/bash -l
#PBS -m abe
#PBS -M yourEmail@griffith.edu.au
#PBS -V
#PBS -N TestContainer
#PBS -q workq
#PBS -l select=1:ncpus=1:mem=2gb,walltime=0:05:00
cd  $PBS_O_WORKDIR

module load singularity                 # load singularity
singularity run pythonContainer.sif     # add the following bash command so that singularity executes the image during runtime
```
{: .bash}

Create a new directory on your HPC account "containerTut"

Copy sif script and data to the hpc. You can copy multiple objects at once by surrounding them in ""
```
nane@name-VirtualBox:~$ scp "directory/on/local/computer/pythonContainer.sif directory/on/local/computer/pythonFile.py"  s1234567@gc-prd-hpclogin1.rcs.griffith.edu.au:~/containerTut
```
{: .bash}


In the HPC, cd into the directory with the singularity data. You should have a scheduler bash script (.sh), a singularity image file (.sif), and a python script (.py). 
Now you can sumbit your singularity image file to the HPC to process

```
[s1234567@gc-prd-hpclogin1 ~]$ cd ./containerTut

[s1234567@gc-prd-hpclogin1 containerTut]$ qsub scheduler.sh

[s1234567@gc-prd-hpclogin1 containerTut]$ ls
pythonContainer.sif        TestContainer.o81400
pythonCont.sh      pythonFile.py           python_primes.csv    TestContainer.e81400

```
{: .bash}


