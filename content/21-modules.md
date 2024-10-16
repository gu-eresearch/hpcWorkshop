---
title: Accessing software and packages
nav: Accessing software and packages
---

By default, modules are not loaded onto our individual work space.
If we want to use a module or package, we will need to "load" it. Note, a module is a software such as R or python, and a package is run within a module (e.g. dplyr in R or pandas in python).

Before we start using modules and packages, 
we should understand the reasoning why they are not auto loaded.
The two biggest factors are incompatibilities and versioning.

Module incompatibility is a major headache for programmers.
Sometimes the presence (or absence) 
of a package will break others that depend on it.
Two of the most famous examples are Python 2 and python 3 and C compiler versions.
Python 3 famously provides a `python` command 
that conflicts with that provided by Python 2. 
Software compiled against a newer version of the C libraries 
and then used when they are not present will result in a nasty
`'GLIBCXX_3.4.20' not found` error, for instance. 

Module versioning is another common issue.
A team might depend on a certain package version for their research project - 
if the module version was to automatically change (for instance, if it was updated),
it might affect their results.
Having access to multiple software versions allow a set of researchers to take 
software version out of the equation if results are weird.

## What modules can we access on the HPC

To see available modules on the HPC, use `module avail` command. Your terminal needs to be logged into the HPC.

```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module avail
```
{: .bash}
```
--------------------------------- /usr/share/Modules/modulefiles ---------------------------------
dot         module-git  module-info modules     null        use.own

---------------------------------------- /etc/modulefiles ----------------------------------------
mpi/mpich-3.2-x86_64

------------------------------------------ /sw/Modules -------------------------------------------
anaconda2/2019.07py2                      library/hdf5/5-1.10.5-intel
anaconda3/2019.07py3                      library/lapack/3.8.0
ansys/v195                                library/loki/0.1.7
bioinformatics/blast+/2.9.0               library/netcdf/netcdf-c/4.7.3
bioinformatics/bowtie2/2.3.5.1            library/netcdf/netcdf-c/4.7.3-intel
bioinformatics/cd-hit/4.8.1               library/netcdf/netcdf-cxx/4.3.1
bioinformatics/celSEQpipeline/1.0         library/netcdf/netcdf-cxx/4.3.1-intel
bioinformatics/fastqc/0.11.8              library/netcdf/netcdf-fortran/4.5.2
bioinformatics/htslib/1.9                 library/netcdf/netcdf-fortran/4.5.2-intel
bioinformatics/orthofinder/2.3.7          library/openblas/0.3.6
bioinformatics/samtools/1.9               library/proj/6.2.1
bioinformatics/signalp/4.1                library/scalapack/2.0.2
bioinformatics/tmhmm/2.0                  library/tcl/8.6.9
bioinformatics/transdecoder/5.5.0         library/tk/8.6.9
bioinformatics/trinity/2.8.6              library/vtk/8.2.0
cmake/3.15.5                              library/xz/5.2.4
cmake/3.8.2                               matlab/2018b
delft3d/6244intel                         matlab/2019b
extras/jags/4.3.0                         misc/aem3d/1.0.1
gaussian/g16                              misc/aem3d/aem3d
gcc/8.3.1                                 misc/automake/1.15
gcc/old/9.2.0                             misc/baronsolver/19.7.13
gcc/old/9.2.0OLD                          misc/cplex/cplex
gromacs/2019.3                            misc/libtool/2.4.6
gromacs/2019.4                            misc/metashape/1.5.2
gromos/md++141                            misc/metashape/1.5.3pro
intel/ics2013                             misc/metis/5.1.0
intel/intelmpi                            misc/qglviewer/2.6.3
intel/intelParallelStudio2019             misc/qglviewer/2.7.1
java/jdk11.0.1                            misc/simbody/361
lang/golang/1.21.0                        misc/yade/2019.01a
lang/golangci-lint/1.21.0                 misc/yasara/yasara
legacy/gcc/4.9.3                          mpi/mpich/3.3.2-gnu
library/boost/1.59.0py3                   mpi/mpich/3.3.2-intel
library/boost/1.70.0py2                   mpi/openmpi/2.1.6
library/boost/1.70.0py3                   mpi/openmpi/4.0.2
library/cgal/4.13.1                       python/2.7.17
library/eigen/3.3.7                       python/3.7.4
library/fftw/3.3.8                        qt/5.12.3
library/fftw/3.3.8mpi                     quantum-espresso/6.4.1
library/gdal/3.0.2                        R/3.6.1
library/geos/3.8.0                        singularity/3.2.0
library/gts/0.7.6                         vasp/vasp541-gnu-openmpi
library/hdf4/4.2.13                       virtualgl/2.6.3
library/hdf5/5-1.10.5
```
{: .output}

If the module you require is not avaliable on the HPC then fill in <a href="https://forms.office.com/Pages/ResponsePage.aspx?id=q8h8Wtykm0-_YGZxQEmtYhZRHEbGuutOhsLljzWVJ1JUQlRTM0lTV0o0TFozWjlVMTQ5TVU0WjhBSC4u" target="_blank">this form</a> and in "Problem Description" write the software/package you would like along with the version number.

## Loading and unloading software

Intially, the statistical program R is not loaded. 

```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ which R
```
{: .bash}
```
no R in (/opt/clmgr/sbin:/opt/clmgr/bin:/opt/sgi/sbin:/opt/sgi/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/c3/bin:/opt/pbs/bin:/sbin:/bin:/export/home/sNumber/bin)
```
{: .output}

Load R into your environment using the `module load` command. If you remember from the scheduler tutorial, we can add module load into the scheduler script.

```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module load R
[s1234567@clogin1.rcs.griffith.edu.au ~]$ R --version
```
{: .bash}
```
R version 3.6.1 (2019-07-05) -- "Action of the Toes"
Copyright (C) 2019 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under the terms of the
GNU General Public License versions 2 or 3.
For more information about these matters see
https://www.gnu.org/licenses/.
```
{: .output}

What if we want a specific version of a program? There are 2 versions of python avaliable though, 2.7.17 and 3.7.4. Lets load python 3.7.4.

```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module load python/3.7.4
[s1234567@clogin1.rcs.griffith.edu.au ~]$ python --version
```
{: .bash}

```
Python 3.7.4
```
{: .output}

Success!!! 

`module load` will add software to your `$PATH`.

To see which modules are loaded we can run the module list command. Note this will not list packages within modules (i.e. it won't show dplyr in R or pandas in python)

```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module list
```
{: .bash}
```
Currently Loaded Modulefiles:
  1) python/3.7.4
```
{: .output}

```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module load R
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module list
```
{: .bash}
```
Currently Loaded Modulefiles:
  1) R/3.6.1   2) python/3.7.4
```
{: .output}

So in this case, the software `R` and `python` were loaded into your HPC workspace.
Let's try unloading the `R` software.

```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module unload R
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module list
```
{: .bash}
```
Currently Loaded Modulefiles:
  1) python/3.7.4
```
{: .output}

So using `module unload` "un-loads" a module along with it's dependencies.
If we wanted to unload everything at once, we could run `module purge` (unloads everything).

```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module purge
```
{: .bash}
```
No Modulefiles Currently Loaded.
```
{: .output}

Note that `module purge` is informative. 
It lets us know that all but a default set of packages have been unloaded
(and how to actually unload these if we truly so desired).

## Software versioning

So far, we've learned how to load and unload software packages.
This is very useful.
However, we have not yet addressed the issue of software versioning.
At some point, 
you will run into issues where only one particular version of some software will be suitable.
Perhaps a key bugfix only happened in a certain version, 
or version X broke compatibility with a file format you use.
In either of these example cases, 
it helps to be very specific about what software is loaded.

Let's examine the output of `module avail` more closely.

```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module avail
```
{: .bash}
```
--------------------------------- /usr/share/Modules/modulefiles ---------------------------------
dot         module-git  module-info modules     null        use.own

---------------------------------------- /etc/modulefiles ----------------------------------------
mpi/mpich-3.2-x86_64

------------------------------------------ /sw/Modules -------------------------------------------
anaconda2/2019.07py2                      library/hdf5/5-1.10.5-intel
anaconda3/2019.07py3                      library/lapack/3.8.0
ansys/v195                                library/loki/0.1.7
bioinformatics/blast+/2.9.0               library/netcdf/netcdf-c/4.7.3
bioinformatics/bowtie2/2.3.5.1            library/netcdf/netcdf-c/4.7.3-intel
bioinformatics/cd-hit/4.8.1               library/netcdf/netcdf-cxx/4.3.1
bioinformatics/celSEQpipeline/1.0         library/netcdf/netcdf-cxx/4.3.1-intel
bioinformatics/fastqc/0.11.8              library/netcdf/netcdf-fortran/4.5.2
bioinformatics/htslib/1.9                 library/netcdf/netcdf-fortran/4.5.2-intel
bioinformatics/orthofinder/2.3.7          library/openblas/0.3.6
bioinformatics/samtools/1.9               library/proj/6.2.1
bioinformatics/signalp/4.1                library/scalapack/2.0.2
bioinformatics/tmhmm/2.0                  library/tcl/8.6.9
bioinformatics/transdecoder/5.5.0         library/tk/8.6.9
bioinformatics/trinity/2.8.6              library/vtk/8.2.0
cmake/3.15.5                              library/xz/5.2.4
cmake/3.8.2                               matlab/2018b
delft3d/6244intel                         matlab/2019b
extras/jags/4.3.0                         misc/aem3d/1.0.1
gaussian/g16                              misc/aem3d/aem3d
gcc/8.3.1                                 misc/automake/1.15
gcc/old/9.2.0                             misc/baronsolver/19.7.13
gcc/old/9.2.0OLD                          misc/cplex/cplex
gromacs/2019.3                            misc/libtool/2.4.6
gromacs/2019.4                            misc/metashape/1.5.2
gromos/md++141                            misc/metashape/1.5.3pro
intel/ics2013                             misc/metis/5.1.0
intel/intelmpi                            misc/qglviewer/2.6.3
intel/intelParallelStudio2019             misc/qglviewer/2.7.1
java/jdk11.0.1                            misc/simbody/361
lang/golang/1.21.0                        misc/yade/2019.01a
lang/golangci-lint/1.21.0                 misc/yasara/yasara
legacy/gcc/4.9.3                          mpi/mpich/3.3.2-gnu
library/boost/1.59.0py3                   mpi/mpich/3.3.2-intel
library/boost/1.70.0py2                   mpi/openmpi/2.1.6
library/boost/1.70.0py3                   mpi/openmpi/4.0.2
library/cgal/4.13.1                       python/2.7.17
library/eigen/3.3.7                       python/3.7.4
library/fftw/3.3.8                        qt/5.12.3
library/fftw/3.3.8mpi                     quantum-espresso/6.4.1
library/gdal/3.0.2                        R/3.6.1
library/geos/3.8.0                        singularity/3.2.0
library/gts/0.7.6                         vasp/vasp541-gnu-openmpi
library/hdf4/4.2.13                       virtualgl/2.6.3
library/hdf5/5-1.10.5
```
{: .output}

We can also see if there is a specific module available

```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module avail R
```
{: .bash}

```
--------------------------------- /sw/Modules ----------------------------------
R/3.6.1 R/4.0.3

----------------------------- /sw/centos7/Modules ------------------------------
R/3.5.1
```
{: .bash}

Let's have a closer look at the `gcc` module.
GCC is a widely used C/C++/Fortran compiler.
Alot of software is dependent on the GCC version, 
and compile or run issues can occur if the wrong version is loaded.
There are three different versions avaliable on the HPC: `gcc/8.3.1`, `gcc/old/9.2.0` and `gcc/old/9.2.0OLD`.
How do we load each copy and which copy is the default?

```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module load gcc
gcc --version
```
{: .bash}
```
gcc (GCC) 9.2.0
Copyright (C) 2019 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
{: .output}


Three things happened here:
- 1) the default copy of GCC was loaded (version 9.2.0), 
- 2) the Intel compilers (which conflict with GCC) were unloaded,
- 3) and software that is dependent on compiler (OpenMPI) was reloaded.
The `module` system turned what might be a super-complex operation into a single command.

So how do we load the non-default copy of a software package?
In this case, the only change we need to make is be more specific about the module we are loading.
There are two GCC modules: `gcc/8.3.1` and `gcc/old/9.2.0OLD`.
To load a non-default module, the only change we need to make to our `module load` command
is to add in the required version number after the `/`.


> ## Load a specific version of GCC
> 
> Load GCC version 7.3.0
>
> > ## Solution
> >
> > ```
> > [s1234567@clogin1.rcs.griffith.edu.au ~]$ module unload gcc
> > ```
> > {: .bash}
> >
> > ```
> > [s1234567@clogin1.rcs.griffith.edu.au ~]$ module load gcc/7.3.0
> > [s1234567@clogin1.rcs.griffith.edu.au ~]$ gcc --version
> > ```
> > {: .bash}
> > 
> > ```
> >gcc (GCC) 7.3.0
> >Copyright (C) 2017 Free Software Foundation, Inc.
> >This is free software; see the source for copying conditions.  There is NO
> >warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
> > ```
> > {: .bash}
> > 
> {: .solution}
{: .challenge}

## Identify the installed packages within a specific modules

First you need to load and initiate R:
```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module load R

[s1234567@clogin1.rcs.griffith.edu.au ~]$ R

R version 3.6.1 (2019-07-05) -- "Action of the Toes"
Copyright (C) 2019 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

  Natural language support but running in an English locale

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.
```
{: .bash}

Then you can see which packages are avaliable. Below is an example of using R to create a list of the packages.

```
> ip <- as.data.frame(installed.packages()[, c(1, 3:4)])
> rownames(ip) <- NULL
> ip <- ip[is.na(ip$Priority), 1:2, drop=FALSE]
> print(ip, row.names=FALSE)

       Package     Version
         abind       1.4-5
       acepack       1.4.1
       anchors       3.0-8
       askpass         1.1
    assertthat       0.2.1
     backports       1.1.5
     base64enc       0.1-3
            BH    1.69.0-1
          brew       1.0-6
         broom       0.5.2
         callr       3.3.2
    cellranger       1.1.0
     checkmate       2.0.0
      classInt       0.4-2
           cli       1.1.0
         clipr       0.7.0
    clisymbols       1.2.0
    ...              ...
```
{: .bash}

Then quit R to get back to the terminal.

```
> q()
Save workspace image? [y/n/c]: n
[s1234567@clogin1.rcs.griffith.edu.au ~]$
```
{: .bash}

Notice that once you run R the adress changes from `[s1234567@clogin1.rcs.griffith.edu.au ~]$` to `>`, signifying that we are no longer in our home directory on the HPC, but are instead in an instance of R on the HPC.

For python:
```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module load python/3.7.4

[s1234567@clogin1.rcs.griffith.edu.au ~]$ python -c 'help("modules")'
```
{: .bash}

Note that we do not run python to identify the packages.

``` 
Please wait a moment while I gather a list of all available modules...

/sw/python/3.7.4/lib/python3.7/site-packages/pandas/compat/__init__.py:117: UserWarning: Could not import the lzma module. Your installed Python is incomplete. Attempting to use lzma compression will result in a RuntimeError.
  warnings.warn(msg)
Cython              asynchat            imghdr              rlcompleter
HTSeq               asyncio             imp                 runpy
__future__          asyncore            importlib           sched
_abc                atexit              importlib_metadata  secrets
_ast                attr                inspect             select
_asyncio            audioop             io                  selectors
_bisect             backports           ipaddress           setuptools
_blake2             base64              ipython_genutils    shelve
_bootlocale         bdb                 itertools           shlex
_bz2                binascii            json                shutil
_codecs             binhex              jsonschema          signal
_codecs_cn          bisect              jupyter             site
...                 ...                 ...                 ...
```
{: .bash}

If the module you require is not avaliable on the HPC then fill in <a href="https://forms.office.com/Pages/ResponsePage.aspx?id=q8h8Wtykm0-_YGZxQEmtYhZRHEbGuutOhsLljzWVJ1JUQlRTM0lTV0o0TFozWjlVMTQ5TVU0WjhBSC4u" target="_blank">this form</a> and in "Problem Description" write the software/package you would like along with the version number.

## Anaconda environments

Anaconda is a distribution of python and R designed specifically for scientific computing. Anaconda contains the conda command, a package and environment manager that aims to manage dependencies and isolate projects. It is really cool, and really useful, for more indepth information have a look <a href="https://towardsdatascience.com/a-guide-to-conda-environments-bc6180fc533" target="_blank">here</a>

There are multiple prebuilt conda environments avaliable on Griffith's HPC that are suited to bioinformatics.

There is a python 2.x and 3.x installation of Anaconda avaliable
```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module avail

anaconda2/2019.07py2
anaconda3/2019.07py3
```
{: .bash}

Load the module of interest and see what environments are avaliable
```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module load anaconda3/2019.07py3 
[s1234567@clogin1.rcs.griffith.edu.au ~]$ conda info --envs
# conda environments:
#
base                  *  /sw/anaconda3/2019.07
3point6                  /sw/anaconda3/2019.07/envs/3point6
R                        /sw/anaconda3/2019.07/envs/R
bioinformatics           /sw/anaconda3/2019.07/envs/bioinformatics
bioinformatics2          /sw/anaconda3/2019.07/envs/bioinformatics2
cellprofiler             /sw/anaconda3/2019.07/envs/cellprofiler
roaryENV                 /sw/anaconda3/2019.07/envs/roaryENV
toxify_env               /sw/anaconda3/2019.07/envs/toxify_env
trinityENV               /sw/anaconda3/2019.07/envs/trinityENV

```
{: .bash}

Load the environment of interest and check the packages and versions avaliable
```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ source activate bioinformatics
(bioinformatics) [s1234567@clogin1.rcs.griffith.edu.au ~]$ conda list

# packages in environment at /sw/anaconda3/2019.07/envs/bioinformatics:
#
# Name                    Version                   Build  Channel
_libgcc_mutex             0.1                        main  
a5pipeline                20150522                      1    vdbwrair
aragorn                   1.2.36                        0    biocore
barrnap                   0.9                           3    bioconda
bcftools                  1.9                  h68d8f2e_9    bioconda
bedtools                  2.29.2               hc088bd4_0    bioconda
blast                     2.9.0           pl526h3066fca_4    bioconda
blast-plus                2.2.31                        0    biocore
bowtie                    1.0.0                         1    bioconda
bowtie2                   2.2.5                         0    bioconda/label/broken
bwa                       0.7.17               hed695b0_7    bioconda
bzip2                     1.0.8                h7b6447c_0  
ca-certificates           2020.1.1                      0  
cairo                     1.16.0            hfb77d84_1002    conda-forge
cd-hit                    4.8.1                h8b12597_3    bioconda
certifi                   2019.11.28               py38_0  
collectl                  4.0.4                         2    bioconda
configparser              4.0.2                    pypi_0    pypi
curl                      7.68.0               hbc83047_0  
...                       ...                         ...    ...
```
{: .bash}

To deactivate the environment
```
[s1234567@clogin1.rcs.griffith.edu.au ~]$ conda deactivate
```
{: .bash}

## Build your own anaconda environment
You can create your own conda environment in your home directory but please make sure that it is not already avaliable. Creating a conda environment requires internet access.

```
# To get internet access from the cluster, run this command:
[s1234567@clogin1.rcs.griffith.edu.au ~]$ source /usr/local/bin/s3proxy.sh

# Load the require anaconda environment, i.e. 2 or 3
[s1234567@clogin1.rcs.griffith.edu.au ~]$ module load anaconda3

# create a new conda environment using the create command. Use the -n flag to give it a name, and tell it the version of python and anaconda to create the environment with
[s1234567@clogin1.rcs.griffith.edu.au ~]$ conda create -n NameOfCondaEnv python=X.X anacondaX

# There will now be a new conda environment with that name 
[s1234567@clogin1.rcs.griffith.edu.au ~]$ conda info --envs

# activate the environment
[s1234567@clogin1.rcs.griffith.edu.au ~]$ source activate NameOfCondaEnv

# Install the packages you would like to use
[s1234567@clogin1.rcs.griffith.edu.au ~]$ conda install -n NameOfCondaEnv [package]

# deactivate the environment (older versions will use source deactivate)
[s1234567@clogin1.rcs.griffith.edu.au ~]$ conda deactivate

# once you have finished with it and will not be using it again you can delete it
[s1234567@clogin1.rcs.griffith.edu.au ~]$ conda env remove -n NameOfCondaEnv
```
{: .bash}