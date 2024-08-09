## File manipulation fun
**[Here](review_awk_and_sed.md) is the link to the awk and sed tutorial pages. Make sure to at least review these before attempting the following exercises**


1. log in to xanadu and make sure you are in your home directory
2. start an interactive session (hint `srun --partition=general --qos=general --mem=2G --pty bash`)
3. make a directory called `file_fun` (hint `mkdir file_fun`)
4. move into `file_fun` (hint `cd file_fun`)
5. copy the files `/core/labs/Wegrzyn/akriti_temp_ramp/vgsales_top50.csv`, `/core/labs/Wegrzyn/akriti_temp_ramp/arabidopsis.gtf`, and `/core/labs/Wegrzyn/akriti_temp_ramp/test.pep` into your current directory `file_fun` (hint `cp [path_to_file] ./`)


## grep

grep (**g**lobal **r**egular **e**xpression **p**rint) searches input files for a search string, and prints the lines that match it. [More comprehensive tutorial](https://www.geeksforgeeks.org/grep-command-in-unixlinux/)

The simplest grep command is `grep [options] "PATTERN" [file]`

The following command matches all instances of "g3" in the file test.pep and prints those lines. The option `-c` tells grep to only print the number of lines with a match. The option `-n` tells grep to print the line numbers of each line containing a matching pattern.
```
grep "g3" test.pep
grep -c "g3" test.pep
grep -n "g3" test.pep
```

[For more information on regex (regular expressions) to match patterns](https://www.gnu.org/software/grep/manual/html_node/Regular-Expressions.html)

**Search how many lines contain `Nintendo` in `vgsales_top50.csv`**

```
grep "Nintendo" vgsales_top50.csv
```
***Only output the number of matches to `Nintendo`***

```
grep -c "Nintendo" vgsales_top50.csv
```

## sed

sed stands for Stream EDitor and it is used to parse and transform text. [tutorial links](review_awk_and_sed.md)

One of the most useful applications of sed is to "find and replace", but you can do much more with sed. You can find a full list of all the commands [here](https://www.gnu.org/software/sed/manual/sed.html#sed-commands-list)

The sed command for substitution or "find and replace" goes `'s/pattern to match/pattern to replace/flags'` The s indicates "substitute" and the `flags` indicates the number of times sed should implement the substitution. In the command below, the `g` indicates to replace **All** instances of *pattern-to-match* with *pattern-to-replace* 

The following command replaces all commas with tabs, changing a comma seperated file into a tab seperated file. Use head to see what the first ten lines of the new file looks like 
```
sed 's/,/\t/g' vgsales_top50.csv > vgsales_top50.tsv
head vgsales_top50.tsv
```
The output can also be **piped** using **\|** to `head` directly so the output prints directly into the terminal rather than saving the output into a file and then using head. This will not store any of your changes to the file, instead, it will pass the standard output of the first command to the standard input of the second command. In the following command, we pipe the output of `sed` directly to the `head` command.

```
sed 's/,/\t/g' vgsales_top50.tsv | head 
```
```



**Now try**
- what happens if you ommit the `g` flag in substitute command? `sed 's/,/\t/' vgsales_top50.tsv | head`
- what happens if you replace the `g` with 5? `sed 's/,/\t/5' vgsales_top50.tsv | head`

Note that sed is case sensitive: 
```
sed 's/Nintendo/NINTENDO/g' vgsales_top50.tsv | head 
```

**Now try converting all instances of the lowercase `exon` into uppercase `EXON` in the file arabidopsis.gtf, pipe to head to see your results.**

<p>
<details>
<summary>hint</summary>
<pre><code>
sed ‘s/exon/EXON/’ arabidopsis.gtf | head
</code></pre>
</details>
</p>

You can also modify the file "in place" with the option `-i`. **What happens to the file arabidopsis.gtf after executing the following command?**
```
sed -i ‘s/transcript:AT/transcript:arabi/’ arabidopsis.gtf
``` 
Another usedful application of sed is removing/deleting lines matching a certain pattern. The following command removes all lines containing "Nintendo", how many games are left?
```
sed '/Nintendo/d' vgsales_top50.tsv
```

**Now try removing all lines matching `"transcript:arabi1G01010.1"` in `arabidopsis.gtf`**

<p>
<details>
<summary>hint</summary>
<pre><code>
sed -i ‘/"transcript:arabi1G01010.1"/d’ arabidopsis.gtf
</code></pre>
</details>
</p>


## awk 
`awk` is a domain-specific language designed for text processing and typically used as a data extraction and reporting tool. It can be used for a variety of applications. [tutorial links](review_awk_and_sed.md)

One of this is viewing or extracting certain columns from a file.

```
awk -F'\t' '{print $3}' arabidopsis.gtf | head
```
Here the option `-F` indicates to `awk` that the file is tab seperated. Another way to specify this would be to use the `BEGIN {FS="\t"}` in the beginning like so. `BEGIN` tells `awk` "before you try to process any lines, do this"
```
awk 'BEGIN{FS="\t"}{print $3}' arabidopsis.gtf | head 
```

You don't have to print columns in order
```
awk 'BEGIN{FS="\t"}{print $4, $5, $1, $3, $2}' vgsales_top50.tsv | head
```
**What order are the columns in?**

You can also use awk to set the field seperator to a comma so you can manipulate the original csv file, and use head to show just the first 10 lines. `awk` outputs space seperated files by default, but you can change that using `OFS=`

**Note the difference between the output of the two commands:**

```
awk 'BEGIN{FS=","}{print $1, $2, $3, $4}' vgsales_top50.csv | head
awk 'BEGIN{FS=","; OFS="\t"}{print $1, $2, $3, $4}' vgsales_top50.csv | head
```  

The second command, if sent to a file rather than being piped to head, would do the same thing as `sed 's/,/\t/g' vgsales_top50.csv > vgsales_top50.tsv`

**Piping the output of `awk`** 
The following command counts the number of times each feature appears in the .gtf file

```
awk -F'\t' '{print $3}' arabidopsis.gtf | sort | uniq -c
```
**How many features are there and how many times does each appear?**

You can also use conditional statements with awk like `if` and `if else`. The following command prints column 9 if the entry in column 3 is `transcript` (`$3 == "transcript"` means if true that column 3 is exactly "transcript", print column 9)
```
awk -F'\t' '$3 == "transcript" { print $9 }' arabidopsis.gtf | head
```
**Now try printing column 9 if column 3 is `gene`**

<p>
<details>
<summary>hint</summary>
<pre><code>
awk -F'\t' '$3 == "gene" { print $9 }' arabidopsis.gtf | head
</code></pre>
</details>
</p>

## other commands

[***sort***](https://www.redhat.com/sysadmin/sort-command-linux)

`sort [file_to_sort]`

[***find***](https://www.redhat.com/sysadmin/linux-find-command)

`find [path_to_search_in] [options] [expression]`

Here the options are `-type f` meaning I only want it to print out files, not directories, and `-name "*txt"` means only print files matching that pattern, meaning any file ending in .txt)

`find /home/FCAM/abhattarai -type f -name "*.txt"` 

[***cut***](https://www.baeldung.com/linux/cut-command)

`cut [options] [file_to_process]`


## [Helpful one-liners](https://github.com/stephenturner/oneliners)
