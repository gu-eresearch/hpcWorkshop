---
title: Running and querying your job
nav: Running jobs
---

## Submitting a job

To submit this job to the scheduler, we use the `qsub` command.

```
[s1234567@gc-prd-hpclogin1 ~]$ qsub example-job.sh
```
{: .bash}

qsub will echo the job id and the host
```
49230.gc-prd-hpcadm
```
{: .output}

The job ID is the numbers before the . In this case the job ID is 49230

And that's all we need to do to submit a job. 

## Check the jobs status

To check the status of all job's, including our own, we use the command `qstat`.

```
[s1234567@gc-prd-hpclogin1 ~]$ qstat
```
{: .bash}
```
Job id            Name             User              Time Use S Queue
----------------  ---------------- ----------------  -------- - -----
5884.gc-prd-hpcad test             s1234567          1416:13: R workq           
7488.gc-prd-hpcad test             s1234567          1153:38: R workq           
8248.gc-prd-hpcad test             s1234567          568:31:5 R workq           
8482.gc-prd-hpcad test             s1234567          433:15:0 R workq           
8524.gc-prd-hpcad test             s1234567          410:52:3 R workq          
```
{: .output}

We can see the details of all jobs, most importantly, we can see that it is in the "R" or "RUNNING" state. It could be in "Q" or "QUEUED" state.

qstat flags
   -a  Display all jobs in any status (running, queued, held)
   -r Display all running or suspended jobs
   -n Display the execution hosts of the running jobs
   -i Display all queued, held or waiting jobs
   -u username: Display jobs that belong to the specified user
   -s Display any comment added by the administrator or scheduler. This option is typically used to find clues of why a job has not started running.
   -f job_id: Display detailed information about a specific job
   -xf job_id, -xu user_id: Display status information for finished jobs (within the past 7 days)


> ## Check the status of just your job
>
> hint use qstat and a flag for your username or job ID
>
> > ## Solution
> >
> > ```
> > [s1234567@gc-prd-hpclogin1 ~]$ qstat -x jobID
> >
> > [s1234567@gc-prd-hpclogin1 ~]$ qstat -u s1234567
> > ```
> > {: .bash}
> {: .solution}
{: .challenge}

## Get all avaliable information about your job
```
[s1234567@gc-prd-hpclogin1 ~]$ qstat -xf 49223
Job Id: 49223.gc-prd-hpcadm
    Job_Name = simple_failing_test
    Job_Owner = s1234567@gc-prd-hpclogin1.ib0.cm.rcs.griffith.edu.au
    resources_used.cpupercent = 0
    resources_used.cput = 00:00:00
    resources_used.mem = 3548kb
    resources_used.ncpus = 1
    resources_used.vmem = 346832kb
    resources_used.walltime = 00:00:49
    job_state = F
    queue = workq
    server = gc-prd-hpcadm
    Checkpoint = u
    ctime = Wed Jun 24 15:33:18 2020
    Error_Path = gc-prd-hpclogin1.ib0.cm.rcs.griffith.edu.au:/export/home/s1234567/simple_failing_test.e49223
    exec_host = gc-prd-hpcn001/3
    exec_vnode = (gc-prd-hpcn001:ncpus=1)
    Hold_Types = n
    Join_Path = n
    Keep_Files = n
    Mail_Points = a
    mtime = Wed Jun 24 15:34:09 2020
    Output_Path = gc-prd-hpclogin1.ib0.cm.rcs.griffith.edu.au:/export/home/s1234567/simple_failing_test.o49223
    Priority = 0
    qtime = Wed Jun 24 15:33:18 2020
    Rerunable = True
    Resource_List.ncpus = 1
    Resource_List.nodect = 1
    Resource_List.place = pack
    Resource_List.select = 1
    Resource_List.walltime = 00:00:30
    stime = Wed Jun 24 15:33:18 2020
    session_id = 53391
    jobdir = /export/home/s1234567
    substate = 93
    Variable_List = PBS_O_SYSTEM=Linux,PBS_O_SHELL=/bin/bash,
	PBS_O_HOME=/export/home/s1234567,PBS_O_LOGNAME=s1234567,
	PBS_O_WORKDIR=/export/home/s1234567,PBS_O_LANG=en_US.UTF-8,
	PBS_O_PATH=/opt/clmgr/sbin:/opt/clmgr/bin:/opt/sgi/sbin:/opt/sgi/bin:/
	usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/c3/bin:/opt/pbs/b
	in:/sbin:/bin:/export/home/s1234567/bin,
	PBS_O_MAIL=/var/spool/mail/s1234567,PBS_O_QUEUE=workq,
	PBS_O_HOST=gc-prd-hpclogin1.ib0.cm.rcs.griffith.edu.au
    comment = Job run at Wed Jun 24 at 15:33 on (gc-prd-hpcn001:ncpus=1) and fa
	iled
    etime = Wed Jun 24 15:33:18 2020
    run_count = 1
    Stageout_status = 1
    Exit_status = 271
    Submit_arguments = fail.sh
    history_timestamp = 1592976849
    project = _pbs_project_default
```
{: .bash}

## Put a job on hold

Only the job owner or a system administrator with "su" or "root" privilege can place a hold on a job. The hold can be released using the qrls command.
```
[s1234567@gc-prd-hpclogin1 ~]$ qhold jobID
```
{: .bash}

```
[s1234567@gc-prd-hpclogin1 ~]$ qrls jobID
```
{: .bash}

## Canceling a job

Sometimes we'll make a mistake and need to delete a job.
This can be done with the `qdel` command.
Let's look at how to submit a job and then delete it using its job number.

```
[s1234567@gc-prd-hpclogin1 ~]$ qsub example-job.sh
49230.gc-prd-hpcadm
```
{: .bash}

```
[s1234567@gc-prd-hpclogin1 ~]$ qstat -x 49230

Job id            Name             User              Time Use S Queue
----------------  ---------------- ----------------  -------- - -----
49230.gc-prd-hpca example-job      s1234567          00:00:00 F workq 
```
{: .output}

Now delete the job with it's job number. 
Absence of any job info indicates that the job has been successfully canceled.

```
[s1234567@gc-prd-hpclogin1 ~]$ qdel 49230
[s1234567@gc-prd-hpclogin1 ~]$ qstat -x 49230

Job id            Name             User              Time Use S Queue
----------------  ---------------- ----------------  -------- - -----

```
{: .output}

## Cancelling multiple jobs

We can also all of our jobs at once using the `-u` option. 
This will delete all jobs for a specific user (in this case us).
Note that you can only delete your own jobs.

Try submitting multiple jobs and then cancelling them all with
``` 
[s1234567@gc-prd-hpclogin1 ~]$ qdel -u s1234567
```
{: .bash}

## Checking for errors and outputs

Once a job has run two new files will be created in your directory, and error and an output file. They will have the name assigned in your scheduler script under the command #PBS -N XXXX, followed by a .e1234 (error) and .o1234 (output).

```
[s1234567@gc-prd-hpclogin1 ~]$ ls
prime_numbers.R     primes.csv     scheduler.sh  primes.e49237  primes.e49237
```
{: .bash}

We can look at them in our UNIX CLI using the more command.

```
[s1234567@gc-prd-hpclogin1 ~]$ more primes.e49237
/var/spool/pbs/mom_priv/jobs/49243.gc-prd-hpcadm.SC: line 10: d: command not found
/var/spool/pbs/mom_priv/jobs/49243.gc-prd-hpcadm.SC: line 15: results.Rout: command not found
```
{: .bash}
