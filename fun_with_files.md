## File name is vgsales.csv and is located in /home/FCAM/ramp

## use pwd to check that you are in your directory /home/FCAM/ramp/usr#

## View the file 
using `less /home/FCAM/ramp/vgsales.csv`, then exit out of less using `q`. Notice that the direct path is given here. 

## Using sed to change csv to tsv, and head to view what it looks like now 
```
sed 's/,/\t/g' /home/FCAM/ramp/vgsales.csv > vgsales.tsv
head vgsales.tsv 
```

## Using awk to select specific columns 
`awk '{print $#, $#, .. }' <filename>` prints out columns from the file \<filename\> 

the command below prints out columns 1-3 of the vgsales.tsv file to a new file called cols3_vgsales.tsv. Use head to see what the file looks like.
```
awk '{print $1,$2,$3}' vgsales.tsv > cols3_vgsales.tsv
head cols3_vgsales.tsv 
```
You don't have to print columns in order
```
awk '{print $4, $5, $1, $3, $2}' vgsales.tsv > mix_cols_vgsales.tsv
head mix_cols_vgsales.tsv
```
What order are the columns in?
