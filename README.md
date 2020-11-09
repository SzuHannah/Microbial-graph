# Microbial-graph project  
## Draft for blogpost 
[goolge doc link](https://docs.google.com/document/d/16YL4hZdoKpR3T1TXYm4Wz2-qHcmJqwhHWDuzH0j7B7k/edit?usp=sharing)  

## Useful links  
ref:  
[emp repo](https://github.com/biocore/emp)  
[emp repo_cooccurence](https://github.com/biocore/emp/tree/master/code/08-cooccurrence-nestedness/cooccurrence-network)  
[emp_sample metadata](https://github.com/biocore/emp/blob/master/data/mapping-files/emp_qiime_mapping_subset_2k.tsv)  
## Process data from .biom to tsv  
1. Install ruby (the emp repo use ruby to convert .biom to tsv)  
2. Run "run.sh" under [1_dataProcessing/bioTocsv](https://github.com/SzuHannah/Microbial-graph/tree/main/1_dataProcessing/biomTotsv)  
- To run "run.sh", directly type ./run.sh in the terminal   
- If typing ./run.sh gives you erro, maybe it's because the script is not executable. Then, try to type the command chmod +x to make the file executable: chmod +x run.sh. After making it executable, type ./run.sh should work  
- run.sh is a bash script, we can edit it accordingly. eg. change the BIOM, or change the MAP file name. If we use the same ones, then no need to change.  
- I commented out the line that computes OTU co-occurence network in run.sh, because I think we can use our own ways(eg. vsp or others to calculate. I tried out vsp, described below). Feel free to recover this line if you want to see their version of OTU co-occurence network. (To recover, remove the "#" symbol in front of the line)    
3. Within your folder, you'll see several files after run.sh finish (here are the resulting files if BIOM=emp_deblur_90bp.subset_2k.rare_5000.biom): column_metadata.txt, emp_deblur_90bp.subset_2k.rare_5000.biom.json, emp_deblur_90bp.subset_2k.rare_5000.biom.SUMMARY.txt, otu-co-occurrence-network-z20.csv, otu_sample_value.txt, row_metadata.txt. If you also comment out the line for otu_cooccurrence_network calculation, there won't be otu-co-occurrence-network-z20.csv, but feel free to play aruond!  
- To do the clustering, we'll mainly need the "otu_sample_value.txt", "row_metadata.txt", "column_metadata.txt"  
  
## From tsv to graph  
If we end up using our own method to cluster(instead of their otu_cooccurrence_network method), perhaps an efficient way is we modify their code to fit our needs.  
1. For now, I tried out with vsp(the package mentioned in stat992). Please see the script "otu_cooccurrence_network_notes.R" under [2_tsvTograph](https://github.com/SzuHannah/Microbial-graph/tree/main/2_tsvTograph).  
2. I've also put the data needed under 2_tsvTograph/, feel free to use!  
- The code can convert the "otu_sample_value.txt" into a sparse bipartite adjacency matrix(row: otu; column: sample).  
- Then we can subset the otus of interest.  
- After subsetting, we can plug this matrix of interest into the vsp function for clustering. This way, we can see the cluster of otus and the samples, by inspecting the Z and Y matrix respectively. Just an idea though, welcome to any thoguht!!!  

