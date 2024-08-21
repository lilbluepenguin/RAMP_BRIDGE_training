Copy the following files with `cp /path/to/file /path/to/destination`
```
cp /core/labs/Wegrzyn/akriti_temp_ramp/Caenorhabditis_elegans.WBcel235.110.chromosome.I.gff3 .
cp /core/labs/Wegrzyn/akriti_temp_ramp/monday.fasta .
cp  /core/labs/Wegrzyn/akriti_temp_ramp/test.pep .
```

Rename files `mv oldname newname`
```
mv Caenorhabditis_elegans.WBcel235.110.chromosome.I.gff3 c_elegans.gff3
mv test.pep test.pep.fasta
mv monday.fasta elegans.cds.fasta 
```

Lets look at the fasta files first 
```
head elegans.cds.fasta
```
***Q. What information do you see in the header for each sequence entry?***

It's looking messy, what if we only want some of the information? You can remove everything after the first space with the following command 
```
cut -d' ' -f 1 elegans.cds.fasta > short_header_elegans.cds.fasta
```

The `-d' '` tells the cut command that you want to delimite by space (default is tab). `-f 1` tells the cut command that you only want to keep the first field of each line. 

***Q. What do you see in the header now?***

***Q. how would you count the number of sequences in your fasta file?***

One thing is you know all header lines start with '>' and each sequence has a header. You can use `grep` to print out all the header lines

```
grep '>' elegans.cds.fasta
grep '>' -c elegans.cds.fasta
```
***Q. What's the difference between the output of the two commands? Which one gives you the information you want?***

***Q. How many sequences are in `test.pep.fasta`?***

***Q. Try the following command, what does it output?***
```
grep '>' -c *.fasta
```

***Q.Count the number of lines in `elegans.cds.fasta`***
```
wc -l 
```



***Q. what happens if you leave out the -l flag?***






