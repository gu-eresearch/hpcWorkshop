---
title: Using Windows Software on A VM
nav: Using Windows Software on A VM
---

The HPC runs on a linux operating system. You can run most software on a linux. However, there are some Windows software the will only run on a Windows operating system, in this case a windows VM will need to be created to run your analysis. The windows VM will run at about the same speed as your computer, maybe a bit quicker, but it will free your computer up to do other things.
You can request your own windows VM or you can request an eresearch managed VM.



## Moving files from your computer to a VM
Windows

Mac
In finder go to "Go" in the index bar, click "Connect to sever" then type "smb://staff.ad.griffith.edu.au/users"
and click "connect". Open your finder window and you will notice a new external drive has appeared.


## Prevent the Windows VM from timing out and stopping your analysis
The Windows VM will only stay open for 15 minutes without activity. It will then close and your analysis will stop running, this is annoying, but its a security feature that cannot be stopped. So you will need to run your analysis as a service.

### Running a R script as a service
First of you will need to set up the path to R in the systems environmental variables on the Windows VM. To do this:
1. Go to the Windows “Search” bar
2. Type “Edit the system environment variables”
3. Click the button “Environment Variables…” at the bottom
4. On the bottom pane, under “System variables”, highlight the “Path” variable and click “Edit”.
5. Click “New” and add the path of the “bin” folder of your R base software (nor Rstudio). The path will look like: C:\Program Files\R\R-3.6.1\bin , although it will change depending on the version of R
6. Click OK in all windows

Now you can go to the Windows “Search” bar and open the windows command prompt. You will need to change directory to where you want to write any output files. Then you can run you script using the 'Rscript' command.

```
H:\>c:

C:\>cd \Users\s1234567\Desktop

C:\Users\s1234567\Desktop> Rscript C:\Users\s1234567\Desktop\scriptLong.R
```
{ .bash}

"C:\Program Files\R\R-3.6.1\bin\Rscript.exe" C:\Users\s1234567\path to file\scriptLong.R
## Running a python script as a service


## By passing the VM rpd timeout
The Windows VM will only stay open for 15 minutes without activity. It will then close and your analysis will stop running, this is annoying, but its a security feature that cannot be stopped.