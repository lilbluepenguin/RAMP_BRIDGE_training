# Working with the Xanadu Cluster: Writing scripts


## command to start an interactive session:
When you first log into Xanadu, you are on the head node or login node. YOU **DO NOT** WANT TO DO ANY WORK HERE. This will slow down the node for **EVERYONE** which we do not want. Instead, you want to start an interactive session using the command `srun` as shown below. You can adjust the memory you want to allocate to this interactive session with the `--mem=` flag. For partition, you will usually use `general`
```
srun --partition=general --qos=general --mem=2G --pty bash
```

## Loading software on Xanadu
Software is globally installed as **Modules**

To see what software is currently installed, use `module avail` 

You should see something like the following, just much longer:
```
---------------------------------------- /isg/shared/modulefiles -----------------------------------------
2.1.1                              gmap/2020-12-17                    perl/5.32.1
4Cin/1.0                           gmap/2021-05-27                    perl/5.36.0
abyss/1.9.0                        gmap/2023-02-17                    PfamScan/1.6
abyss/2.0.2                        GMcloser/1.6.2                     pfamtools/2017-02-23
abyss/2.1.2                        gmp/6.2.1                          phobius/1.01
abyss/2.1.4                        gnu-parallel/20160622              phrap/1.090518
abyss/2.3.5                        gnuplot/5.2.2                      phred/0.071220.c
add_scores/v1                      gos/0.1.1                          phylip/3.697
...
```

You can see above that there are different versions for most software, so make sure to specify which version you want to use when you load the module. You can activate modules using the command `module load <module name>`

Here I want to see all the versions of R installed on the cluster so I use `module avail R/` and here is the output:
```
---------------------------------------- /isg/shared/modulefiles -----------------------------------------
R/2.14.2   R/3.2.1    R/3.3.2    R/3.4.3    R/3.5.2    R/3.6.3    R/4.2.0
R/3.1.0    R/3.2.3    R/3.3.3    R/3.5.1    R/3.6.0    R/4.0.3    R/4.2.2
R/3.1.2    R/3.3.1    R/3.4.1    R/3.5.1-MS R/3.6.1    R/4.1.2
```
I want to load the most recent installation, which I now know is version 4.2.2, using the command `module load R/4.2.2`

You can check what modules are currently loaded using `module list`
```
Currently Loaded Modulefiles:
  1) R/4.2.2
```
You can unload modules using the `module unload <module name>`

## Starting a script

We will be using the command line text editor nano

```
nano test_script.sh
```

To close out of a file, use `control + x`

It will prompt you if you want to save the edits and also what name you want to save the file under. The current file name should already be populated, but you could choose to edit a file with nano and instead of saving the edits to the original, save it under a different name and it will not overwrite the original file.

## Setting up your scripts to run on Xanadu - SLURM scheduler 
This is what the SLRUM header looks like. The header is what tells the job manager on Xanadu (SLRUM) important information information in order to schedule your job.

This includes 
- job name (something descriptive)
- number of cpus
- amount of memory (RAM)
- partition
- qos 
- email notifications
- error and out files to write  information to 

```
#!/bin/bash
#SBATCH --job-name=test_job
#SBATCH -N 1
#SBATCH -n 1
#SBATCH -c 1
#SBATCH --partition=general
#SBATCH --qos=general
#SBATCH --mail-type=END
#SBATCH --mem=1G
#SBATCH --mail-user=
#SBATCH -o %x_%j.out
#SBATCH -e %x_%j.err
```
In the last two lines of the header:
- the %x stands for the slurm job name 
- the %j stands for the slurm jobid that is assigned upon submission
- therefore `%x_%j` prints to the prefix `jobname_jobID`
- you can also choose to write specific names for the error and out files rather than using the job name. I would always include the `%j` so that each time you run your job you don't overwrite your err and out files

```
#SBATCH -o newjob_%j_.out
#SBATCH -e newjob_%j.err
```

## command to submit a script:
```
sbatch [scriptname]
```

## command to check on jobs in your queue:
```
squeue
```
It should look something like this, but with all jobs across users. If you want only your job use `squeue -u [username]`:
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


## Practice
**part 1: using modules**
- start an interactive session using `srun`, make sure to check that you are in fact in an interactive session and not on one of the head nodes using the command `hostname`. You should see something like `xanadu-##`

- use the command `module avail` to see all of the modules available

- load the module `seqkit/2.2.0` using `module load [modulename]`

- use `module list` to check that the module is loaded *(optional: type `seqkit` and see what information it prints out about the software)*
  
- use `module unload [modulename]` to unload the module

**part 2: writing scripts**
- create a file named `test_script.sh`
  
- open `test_script.sh` using `nano`
  
- paste in the default SLURM header
<p>
<details>
<summary>default header</summary>
<pre><code>
#!/bin/bash
#SBATCH --job-name=test_job
#SBATCH -N 1
#SBATCH -n 1
#SBATCH -c 1
#SBATCH --partition=general
#SBATCH --qos=general
#SBATCH --mail-type=END
#SBATCH --mem=1G
#SBATCH --mail-user=
#SBATCH -o %x_%j.out
#SBATCH -e %x_%j.err
</code></pre>
</details>
</p>

- make sure the partition is `general` and qos is `general`
  
- edit the header to reflect 1 cpu and 1 GB of memory if not already

- add your email 
  
- in your script, after the last line of the header, type
    ```
    echo "hostname: `hostname`"
    echo "date: `date`"

    echo "this is a new file" > newfile.txt
    ```
- submit your script using `sbatch`
  
- check on your running jobs with `squeue -u [username]`
  
- Your job might finish before you even get a chance to check, so check on the recently completed job using `sacct`
  
- check on the resources used using `seff`


***Running a real job***
- create a file called `fastqc.sh`

- copy the file `/core/labs/Wegrzyn/ConGenExample/PTK1_mRNASeq_S27_R1.fastq.gz` to your home directory

- open `fastqc.sh` with nano and paste the following script
<p>
<details>
<summary>fastqc.sh</summary>
<pre><code>
#!/bin/sh
#SBATCH --job-name=JobName
#SBATCH -N 1
#SBATCH -n 1
#SBATCH -c 12
#SBATCH --partition=general
#SBATCH --qos=general
#SBATCH --mail-type=end
#SBATCH --mem=50G
#SBATCH --mail-user=first.last@uconn.edu
#SBATCH -o %x_%j.out
#SBATCH -e %x_%j.err

echo `hostname`

module load fastqc/0.11.7 

fastqc PTK1_mRNASeq_S27_R1.fastq.gz

</code></pre>
</details>
</p>
  
- edit the script to have your email in place of `first.last@uconn.edu` in the SLURM header

- change the job name to "test_fastqc"

- change the number cores to 6

- change the memory requested to 25G
  
- submit the example script `fastqc.sh` using `sbatch`
  
- check on the running job using `squeue -u [username] `, if it is still in the queue (says PD for job status) wait a minute and type `squeue -u [username]` again to see if it has started running

- check on the resources used by the job after it's done running using `seff`

- to transfer the file to your local computer to open the HTML file in your web browser use terminal or powershell to navigate to the folder you want to download to on your local computer. Then use the command below to use scp to copy the file
  ```
  scp [user]@transfer.cam.uchc.edu:/home/FCAM/user/path/PTK1_mRNASeq_S27_R1_fastqc.html ./
  ```
