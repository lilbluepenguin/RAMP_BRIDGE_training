# Working with the Xanadu Cluster: Writing scripts

## Starting a script

We will be using the command line text editor nano

```
nano test_script.sh
```

To close out of a file, use `control + x`

It will prompt you if you want to save the edits and also what name you want to save the file under. The current file name should already be populated, but you could choose to edit a file with nano and instead of saving the edits to the original, save it under a different name and it will not overwrite the original file.




## This is what the SLRUM header looks like:
The header is what tells the job manager on Xanadu (SLRUM) important information information in order to schedule your job.
This includes 
- job name (something descriptive)
- number of cpus
- amount of memory 
- partition
- qos 
- email notifications
- error and out files to write  information to 

```
#!/bin/bash
#SBATCH --job-name=test_job
#SBATCH -N 1
#SBATCH -n 1
#SBATCH -c 4
#SBATCH --partition=general
#SBATCH --qos=general
#SBATCH --mail-type=END
#SBATCH --mem=10G
#SBATCH --mail-user=
#SBATCH -o %x_%j.out
#SBATCH -e %x_%j.err
```

- the %x stands for the slurm job name 
- the %j stands for the slurm jobid that is assigned upon submission
- you can also choose to write specific names for the error and out files like so:


```
#SBATCH -o test_job.out
#SBATCH -e test_job.err
```

## command to submit a script:
```
sbatch [scriptname]
```

## command to check on jobs in your queue:
```
squeue
```
It should look something like this:
```
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
7009065   general stringti abhattar PD       0:00      1 (Priority)
7008932   general     bash abhattar  R       4:47      1 xanadu-29
```

## command to check on job accounting data for your jobs: 
```
sacct
```
It should look something like this:
```
       JobID    JobName  Partition    Account  AllocCPUS      State ExitCode 
------------ ---------- ---------- ---------- ---------- ---------- -------- 
7003443      stringtie2    general pi-wegrzyn          8  COMPLETED      0:0 
7003443.bat+      batch            pi-wegrzyn          8  COMPLETED      0:0 
7003443.ext+     extern            pi-wegrzyn          8  COMPLETED      0:0 
7008932            bash    general pi-wegrzyn          1    RUNNING      0:0 
7008932.ext+     extern            pi-wegrzyn          1    RUNNING      0:0 
7008932.0          bash            pi-wegrzyn          1    RUNNING      0:0 
```


## command to check on resources a job used - this will only be accurate after the job finishes running
```
seff [jobid] 
```

it should look something like this:
```
Job ID: 6999333
Cluster: xanadu
User/Group: abhattarai/wegrzynlab
State: COMPLETED (exit code 0)
Nodes: 1
Cores per node: 16
CPU Utilized: 04:20:16
CPU Efficiency: 75.13% of 05:46:24 core-walltime
Job Wall-clock time: 00:21:39
Memory Utilized: 11.78 GB
Memory Efficiency: 14.73% of 80.00 GB
```

## command for cancelling a job:
```
scancel [jobid]
```

## command to see info about cluster nodes:
```
sinfo 
```

<p>
<details>
<summary>Information from sinfo </summary>
<pre><code>
PARTITION  AVAIL  TIMELIMIT  NODES  STATE NODELIST
general*      up   infinite      1  drng@ xanadu-57
general*      up   infinite     32    mix xanadu-[02-03,05,20-21,23-27,29,34,36,39,49,51-53,58-64,66-67,69-70,72-74]
general*      up   infinite      9  alloc xanadu-[01,04,08,10,22,30-31,54,65]
general*      up   infinite      3   idle xanadu-[46-47,50]
vcell         up   infinite      4    mix xanadu-[76-78,80]
vcell         up   infinite      3   idle xanadu-[79,81-82]
vcellpu       up   infinite      1   idle xanadu-32
himem         up   infinite      5    mix xanadu-[06,40-43]
himem         up   infinite      1   idle xanadu-44
himem2        up   infinite      2    mix xanadu-[07,75]
xeon          up   infinite      1  drng@ xanadu-57
xeon          up   infinite     19    mix xanadu-[02-03,05,39,49,51-53,58-64,66-67,69-70]
xeon          up   infinite      4  alloc xanadu-[04,08,54,65]
xeon          up   infinite      3   idle xanadu-[46-47,50]
amd           up   infinite     10    mix xanadu-[20-21,23-27,29,34,36]
amd           up   infinite      4  alloc xanadu-[10,22,30-31]
mcbstudent    up   infinite      1    mix xanadu-68
mcbstudent    up   infinite      1   idle xanadu-71
gpu           up   infinite      5    mix xanadu-[02-03,05-07]
gpu           up   infinite      3  alloc xanadu-[01,04,08]
gpu           up   infinite      2   idle xanadu-[84-85]
special       up   infinite      1  drng@ xanadu-57
special       up   infinite     20    mix xanadu-[51-53,56,58-64,66-67,69-70,72-75,80]
special       up   infinite      2  alloc xanadu-[54,65]
special       up   infinite      5   idle xanadu-[50,55,79,81-82]
crbm          up   infinite      1    mix xanadu-56
crbm          up   infinite      1   idle xanadu-55
</code></pre>
</details>
</p>

## command to start an interactive session:
You can adjust the memory you want to allocate to this interactive session with the `--mem=` flag
```
srun --partition=[partition] --qos=[qos] --mem=2G --pty bash
```


## Practice
- create a file named `test_script.sh`
- open test_script.sh using `nano`
- paste in the default SLURM header 
- set partition and qos to `mcbstudent`
- edit the header to reflect 1 cpu and 1 GB of memory
- set the error file name to test_error.err 
- set the out file name to test_output.out
- in your script, after the last line of the header, type
    ```
    echo "hostname: `hostname`"
    echo "date: `date`"
    touch newfile.txt
    ```
- submit your script using `sbatch` 
- check on your running jobs with `squeue` 
- Your job might finish before you even get a chance to check, so check on the recently completed job using `sacct`
- check on the resources used using `seff` 
- create a file called `example.sh` and copy paste the following script
    <p>
    <details>
    <summary>click to view and copy paste this into the file example.sh using nano</summary>

    <pre><code>PASTE LOGS HERE</code></pre>
    
    </details>
    </p>
- submit the example script `example.sh` using `sbatch`
- check on the running job using `squeue`
- cancel the job submitted with the example script using `scancel [jobid]`
- check on your jobs again using `sacct`
- check on the resources used by the job using `seff`



### Additional Information
If you prefer, you can use a file transfer software and text editor on your computer to edit files. 
