# Running BLAST on the command line

Yesterday I had you follow a tutorial to run BLAST on the NCBI servers. Today we're going to run it on the command line, with custom databases.

First log onto the cluster using `ssh username@xanadu-submit-ext.cam.uchc.edu`

Then start an interactive session using `srun --partition=general --qos=general --mem=2G --pty bash`


The first step is downloading the sequences you want to make the database to search and your query sequences, which I have located at `/core/labs/Wegrzyn/akriti_temp_ramp/blast_example/fastafiles/`
- make a directory called "blast_example"
  ```
  mkdir blast_example
  ```
  
- enter the directory
  ```
  cd blast_example
  ```
  
- To download all of the files use the command
  ```
  cp /core/labs/Wegrzyn/akriti_temp_ramp/blast_example/fastafiles/*.fasta .
  ```
- ***Q. what command would you use to see what files are in your current directory?***


## CUSTOM DATABASE
Once you have the data downloaded, the next step is to make your custom blast database. We are going to submit this in a script. Remember the default header for SLURM 

```
#!/bin/bash
#SBATCH --job-name=
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
- Start a script named submit_makedb.sh
  ```
  nano submit_makedb.sh
  ```
- Paste in the default header
- Set your header to request **2 cores**, **8GB of memory**, name the job makeblastdb, and add your email.
- Under the header, paste in the following code
  ```
  echo `hostname`
  
  module load blast/2.13.0
  
  makeblastdb -in juglans_sequence.fasta -dbtype prot
  makeblastdb -in araport11_cds_repseq.fasta -dbtype nucl
  ```
- close out of nano and save your file
- submit the job
  ```
  sbatch submit_makedb.sh
  ```
- check on your job
  ```
  squeue -u [username]
  ```
- ***Q. After the job finishes how would you check how much of the requested resources were used?***
- ***Q. What did the makeblastb command do?*** (hint: look at the files in your directory)

## SUBMITTING BLAST RUNS

Now that we have our databases we can actually run the aligner!

- Start a script named submit_align.sh
  ```
  nano submit_align.sh
  ```
- Paste in the default header
- Set your header to request **2 cores**, **8GB of memory**, name the job blast_alignments, and add your email.
- Under the header, paste in the following code
```
echo `hostname`
module load blast/2.13.0

blastn -num_threads 2 -query unknown_nuc_seq.fasta -db araport11_cds_repseq.fasta -out unknown_seq_blast_standard_output.txt
blastn -num_threads 2 -query unknown_nuc_seq.fasta -db araport11_cds_repseq.fasta -outfmt "6" -out unknown_seq_blast_tabular.txt

blastp -num_threads 2 -query queryseq.fasta -db juglans_sequence.fasta -out juglans_standard_output.txt 
blastp -num_threads 2 -query queryseq.fasta -db juglans_sequence.fasta -outfmt "6" -out juglans_tabular.txt 
```
- submit the job
  ```
  sbatch submit_align.sh
  ```
- check on your job
  ```
  squeue -u [username]
  ```
- ***Q. Check your result files, what do you see?***
    - what is the difference between standard output and tabular format?
    - what happened for the unknown nucleotide sequence?
    - what might you do differently?
 



