## Daily Challenge Monday August 7th 

Todays challenge will be going over simple linux commands to move around within the cluster, read and manipulate files. 

To start, log in to the Xanadu cluster using the following command and enter your password when prompted:
```
ssh <yourusername>@xanadu-submit-ext.cam.uchc.edu 
```

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
12. 
13. with `ls` the flag `-l` prints long form, so information like the owner, filesize, and group the file belongs to. `ls -lh` prints the memory in a human readable format
14. navigate to the "monday" directory again, your file "monday.fasta" should be in this directory.
15. view the contents of the file "monday.fasta" using `head`, `tail`, and `less`.
16. use `head` and `tail` again but use the -n flag to specify a different number of lines from the default
17. copy the file "monday.fasta" but rename it to "monday.txt" using `cp monday.fasta ./monday.txt`
18. copy the file "monday.fasta" using `cp` but rename it to "tuesday.txt", and then remove it using the `rm` command.  