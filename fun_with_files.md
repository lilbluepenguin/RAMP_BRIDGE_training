## File name is vgsales.csv and is located in /home/FCAM/ramp

## use pwd to check that you are in your directory /home/FCAM/ramp/usr#

## start an interactive session

## View the file 
using `less /home/FCAM/ramp/vgsales.csv`, then exit out of less using `q`. Notice that the direct path is given here. 

## Using sed to change csv to tsv, and head to view what it looks like now 

Generally sed is used for string substitutions in files

the sed command for substitution goes `'s/*pattern to match*/*pattern to replace*/g'` the s indicates "substitute" and the g indicates "global" meaning replace **All** instances of *pattern to match* with *pattern to replace*

Use head to see what the first ten lines of the new file looks like 

The following replaces all commas with tabs, changing a comma seperated file into a tab seperated file
```
sed 's/,/\t/g' /home/FCAM/ramp/vgsales.csv > vgsales.tsv
head vgsales.tsv
```
## more stuff with sed 
sed is case sensitive 
```
sed 's/Nintendo/NINTENDO/g' vgsales.tsv > NINTENDO_vgsales.tsv
head NINTENDO_vgsales.tsv 
```

## Using awk to select specific columns 
`awk '{print $#, $#, .. }' <filename>` prints out columns from the file \<filename\> 

the command below prints out columns 1-3 of the vgsales.tsv file to a new file called cols3_vgsales.tsv. Use head to see what the file looks like.

the `BEGIN {FS="\t"}` sets the file seperator (FS) as `tabs` so that awk knows not to seperate based on spaces, only tabs

```
awk 'BEGIN{FS="\t"}{print $1,$2,$3}' vgsales.tsv > cols3_vgsales.tsv
head cols3_vgsales.tsv 
```
You don't have to print columns in order
```
awk 'BEGIN{FS="\t"}{print $4, $5, $1, $3, $2}' vgsales.tsv > mix_cols_vgsales.tsv
head mix_cols_vgsales.tsv
```
**What order are the columns in?**

You can also use awk to set the field seperator to a comma so you can manipulate the original csv file, and use head to show just the first 10 lines. Awk outputs space seperated files by default, but you can change that using `OFS=`

**Note the difference between the output of the three commands:**

Here, the output of awk is **piped** using **\|** to head directly so the output prints directly into the terminal rather than saving the output into a file and then using head. This will not store any of your changes.
```
awk 'BEGIN{FS=","}{print $1, $2, $3, $4}' /home/FCAM/ramp/vgsales.csv | head
awk 'BEGIN{FS=","; OFS="\t"}{print $1, $2, $3, $4}' /home/FCAM/ramp/vgsales.csv | head
awk 'BEGIN{FS=","; OFS="\t"}{print $1, $2, $3, $4}' /home/FCAM/ramp/vgsales.csv | head

```  


