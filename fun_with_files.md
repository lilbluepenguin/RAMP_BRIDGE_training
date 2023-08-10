## File manipulation fun
**[Here](review_awk_and_sed.md) is the link to the awk and sed tutorial pages. Make sure to at least review these before attempting the following exercises**


1. log in to xanadu and make sure you are in your home directory `/home/FCAM/ramp/usr#`
2. start an interactive session 
3. make a directory called `file_fun`
4. move into `file_fun`
5. copy the files `/home/FCAM/ramp/files/vgsales50.csv`, `/home/FCAM/ramp/files/arabidopsis.gtf`, and `/home/FCAM/ramp/files/test.pep` into your current directory `file_fun`

## sed

sed stands for Stream EDitor and it is used to parse and transform text. 

One of the most useful applications of sed is to "find and replace", but you can do much more with sed. You can find a full list of all the commands [here](https://www.gnu.org/software/sed/manual/sed.html#sed-commands-list)

The sed command for substitution or "find and replace" goes `'s/pattern to match/pattern to replace/flags'` The s indicates "substitute" and the `flags` indicates the number of times sed should implement the substitution. In the command below, the `g` indicates to replace **All** instances of *pattern-to-match* with *pattern-to-replace* 

The following command replaces all commas with tabs, changing a comma seperated file into a tab seperated file. Use head to see what the first ten lines of the new file looks like 
```
sed 's/,/\t/g' vgsales50.csv > vgsales50.tsv
head vgsales50.tsv
```
The output can also be **piped** using **\|** to head directly so the output prints directly into the terminal rather than saving the output into a file and then using head. This will not store any of your changes to the file, instead, it will pass the standard output of the first command to the standard input of the second command. In the following command, we pipe the output of sed directly to the head command.

```
sed 's/,/\t/g' vgsales50.csv | head 
```

Now try 
- what happens if you ommit the `g` flag in substitute command? 
- what happens if you replace the `g` with 5?

Note that sed is case sensitive: 
```
sed 's/Nintendo/NINTENDO/g' vgsales50.tsv | head 
```

Now try converting all instances of the lowercase `exon` into uppercase `EXON` in the file arabidopsis.gtf, pipe to head to see your results rather than saving it as a new file.

You can also modify the file "in place" with the option `-i`. **What happens to the file arabidopsis.gtf after executing the following command?**
```
sed -i ‘s/transcript:AT/transcript:arabi/’ arabidopsis.gtf
``` 

## awk 
AWK is a domain-specific language designed for text processing and typically used as a data extraction and reporting tool. It can be used for a variety of applications.

One of this is viewing or extracting certain columns from a file.

```
awk -F'\t' '{print $3}' arabidopsis.gtf | head
```
Here the option `-F` indicates to `awk` that the file is tab seperated. Another way to specify this would be to use the `BEGIN {FS="\t"}` in the beginning like so 
```
awk 'BEGIN{FS="\t"}{print $3}' arabidopsis.gtf | head 
```

You don't have to print columns in order
```
awk 'BEGIN{FS="\t"}{print $4, $5, $1, $3, $2}' vgsales50.tsv | head
```
**What order are the columns in?**

You can also use awk to set the field seperator to a comma so you can manipulate the original csv file, and use head to show just the first 10 lines. Awk outputs space seperated files by default, but you can change that using `OFS=`

**Note the difference between the output of the two commands:**

```
awk 'BEGIN{FS=","}{print $1, $2, $3, $4}' vgsales50.csv | head
awk 'BEGIN{FS=","; OFS="\t"}{print $1, $2, $3, $4}' vgsales50.csv | head
```  



