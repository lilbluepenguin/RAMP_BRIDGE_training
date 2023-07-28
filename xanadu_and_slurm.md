# Working with the Xanadu Cluster: Writing script

## My preferred way to start a script:

```
nano test_script.sh 
```
--vim is my nemesis and i don't know how to use it 

> insert image of nano screen 


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
#SBATCH --job-name=align_cr1
#SBATCH -N 1
#SBATCH -n 1
#SBATCH -c 16
#SBATCH --partition=general
#SBATCH --qos=general
#SBATCH --mail-type=END
#SBATCH --mem=50G
#SBATCH --mail-user=thewormpit@gmail.com
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

## command to check on job accounting data for your jobs:
```
sacct
```

## command to check on resources a job used
```
seff [jobid] 
```

## command for cancelling a job:
```
scancel [jobid]
```

## command to see info about cluster nodes:
```
sinfo 
```

## command to start an interactive session:
```
srun --partition=[partition] --qos=[qos] --mem=2G --pty bash
```


## Practice
- create a file named test_script.sh
- open test_script.sh 
- paste in the SLURM header 
- set partition and qos to ?mcbstudent? ?general?
- edit the header to reflect 1 cpu and 1 GB of memory
- set the error file name to test_error.err 
- set the out file name to test_output.out
- in your script, after the last line of the header, type ` echo "Hello World my name is ___. This is a SLURM script`
- submit your script using `sbatch` 
- check on your running jobs with `squeue` (your job will likely finish before you even get a chance to check, so it might be empty)
- check on the recently completed job using `sacct`
- check your error and output files, your out file should say "Hello World my name is ___. This is a SLRUM script"
- submit the example script `example.sh` using `sbatch  `
- cancel the job submitted with the example script using `scancel [jobid]`


### Additional Information
If you prefer, you can use a file transfer software and text editor on your computer to edit files. 
