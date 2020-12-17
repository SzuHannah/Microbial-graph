## FAPROTAX workflow
[FAPROTAX database official documentation](http://www.loucalab.com/archive/FAPROTAX/lib/php/index.php?section=Instructions)

My workflow of mapping microbial function to tax info with Faprotax database:  
(1) Download [FAPROTAX latest package](http://www.loucalab.com/archive/FAPROTAX/lib/php/index.php?section=Download), and unzip the folder  
  
(2) Tidy up your dataset into a form such that it can be red by the python script (collapse_table.py) [available form was introduced in 
[this official document](http://www.loucalab.com/archive/FAPROTAX/lib/php/index.php?section=Instructions), and examples were provided in the bottom of the page]. 
In our case, we originally had multiple columns (Kingdom, Phylum, ...) for taxonomy path, so I united these columns into one column called "taxonomy", 
and created a data set that only had two columns: ID and taxonomy, and comment out the column name line (if you did not comment out the column name, 
you'll need to modify the command in step (4)). The tidy input data set was shown here (name: [tax_table.tsv](https://raw.githubusercontent.com/SzuHannah/Microbial-graph/main/4_faprotax/tax_table.tsv)). 
The form matched with the "Example 01" in the official document.   
  
(3) For convenience, move the tax_table.tsv into the same folder as collapse_table.py, which is usually in the FAPROTAX package folder. 
For example, my unzipped FAPROTAX package folder was named FAPROTAX_1.2.4, and it contained the collapse_table.py; 
thus, I put the tax_table.tsv under FAPROTAX_1.2.4 too.  
  
(4) Then, run the command below: (replace tax_table.csv with your file name, 
and replace taxonomy with your column name; specifically, the column that contains taxonomy path info)
```cmd
collapse_table.py -i otu_table.tsv -o tax_table.tsv -g FAPROTAX.txt -c "#" -d "taxonomy" --omit_columns 0 --column_names_are_in last_comment_line -r report.txt -n columns_after_collapsing -v 
```
  
(5) It'll then output a [report.txt](https://raw.githubusercontent.com/SzuHannah/Microbial-graph/main/4_faprotax/report.txt) and functional_table.tsv. 
I only used the report.txt, which required further tidy-up works (functional_table.tsv should be a summary, but it didn't work for me, maybe it'll work for you; 
then, you can just use the functional_table.tsv directly). I read report.txt into python pandas, and omitted the unnecessary columns/rows and duplicate rows. 
Finally pivot the data into a table (called: [seq_func.csv](https://raw.githubusercontent.com/SzuHannah/Microbial-graph/main/4_faprotax/seq_func.csv)) that contains two columns: 
taxonomy path and function.   
  
(6) Finally, read seq_func.csv back to R, and joined with the OTU table by taxonomy info. 
