## Daily Challenge Monday August 7th 

Todays challenge will be going over simple linux commands to move around within the cluster, read and manipulate files. 

To start, log in to the Xanadu cluster and enter your password when prompted, also make sure to start an interactive session and check that you are in an interactive session by typing `hostname` and checking that it looks like `xanadu-##`. If you have forgotten how to start an interactive session, check the [xanadu and slurm](xanadu_and_slurm.md) page and make sure to use mcbstudent for both the partition and qos. 

1. make sure you are in your home directory using  `pwd`

2. list the files in your current directory using `ls`

3. make a new subdirectory called "monday" using `mkdir`

4. enter the subdirectory "monday" using `cd`

5. make three files in this subdirectory using `touch`, make sure to use different names for the file, like test1.txt, test2.txt, test3.txt, or whatever you choose to name them.

6. use `ls` to list the files in the current directory, "monday", you should see the files you just made. 

8. navigate back up to your home directory using `cd` and the relative path `../`

9. download the file "monday.fasta" from this github repository and use your FTP client (FileZilla or Cyberduck) to upload this fasta file to the directory "monday"

10. while in your home directory, list the contents of the subdirectory "monday" by using `ls monday/`. You should now see the file "monday.fasta" in the directory.

11. from anywhere, you can list the contents of your home directory using `ls ~` as the `~` represents your home directory.

12. with `ls` the flag `-l` prints long form, so information like the owner, filesize, and group the file belongs to. `ls -lh` prints the memory in a human readable format

13. navigate to the "monday" directory again, your file "monday.fasta" should be in this directory.

14. view the contents of the file "monday.fasta" using `head`, `tail`, and `less`.

15. use `head` and `tail` again but use the -n flag to specify a different number of lines from the default

16. copy the file "monday.fasta" but rename it to "monday.txt" using `cp <file_to_copy> ./<copied_file_new_name>`

17. copy the file "monday.fasta" using `cp` but rename it to "tuesday.txt" using `mv`

18. change the permissions on the file "tuesday.txt" to be read, write, and executable by all users. Check the permissions using `ls -l`, they should look like `-rwxrwxrwx`. After checking that the permissions are correct, remove the file using the `rm` command.

19. count the number of sequences in the file using `grep` and the proper flag for counting number of instances (hint for the pattern you want to search for, all sequence headers start with the character `>`)

20. using nano open a new file called `page1.txt` and paste in the following text. Then use `grep` to count the number of instances of the words hobbit, on, hole, was, were, he, it.

```
In a hole in the ground there lived a hobbit.
Not a nasty, dirty, wet hole, filled with the ends of worms and an oozy smell, nor yet a dry, bare, sandy hole with nothing in it to sit down on or to eat: it was a hobbit-hole and that means comfort.
It had a perfectly round door like a porthole, painted green, with a shiny yellow brass knob in the exact middle.
The door opened on to a tube-shaped hall like a tunnel: a very comfortable tunnel without smoke, with panelled walls, and floorstiled and carpeted, provided with polished chairs, and lots and lots of pegs forhats and coats - the hobbit was fond of visitors.
The tunnel wound on and on,going fairly but not quite straight into the side of the hill - The Hill, as all thepeople for many miles round called it - and many little round doors opened outof it, first on one side and then on another.
No going upstairs for the hobbit:bedrooms, bathrooms, cellars, pantries (lots of these), wardrobes (he had wholerooms devoted to clothes), kitchens, dining-rooms, all were on the same floor,and indeed on the same passage.
The best rooms were all on the left-hand side(going in), for these were the only ones to have windows, deep-set roundwindows looking over his garden and meadows beyond, sloping down to theriver.
This hobbit was a very well-to-do hobbit, and his name was Baggins.
```

20. Wooo now lets apply this to something biologically related. Make sure we are still in the subdirectory "monday". Paste the example SLURM header from [xanadu and slurm](xanadu_and_slurm.md) into a file called `ensembledownload.sh`, change the jobname to `download_gff3` and give the job 4GB of memory and leave it at 1 CPU. In the body of the script, type:
```
hostname

wget https://ftp.ensembl.org/pub/release-110/gff3/caenorhabditis_elegans/Caenorhabditis_elegans.WBcel235.110.chromosome.I.gff3.gz
gunzip Caenorhabditis_elegans.WBcel235.110.chromosome.I.gff3.gz
```
You should end up with a file called `Caenorhabditis_elegans.WBcel235.110.chromosome.I.gff3` 

21. Use `grep` to create a file with all the lines matching the word "gene". What do you notice about this file? 

(hint, to pipe the output of the `grep` command to a file: `grep "searchterm" filename.txt > newfile.txt`)

22. Count the number of times the following patterns appear in the gff3 file:
      - "RNA"
      - "mRNA"
      - "snoRNA"
      - "biotype=protein_coding"
      - "ID=gene:"
      - "tag=Ensembl_canonical"
      - "description=ncRNA"
