# Collinearity using MCScan

## Preparation of files for MCScan

### Concatenate ortholog-coortholog files from orthomcl
``` cat orthologs.txt coorthologs.txt > ortho-coortho.txt```

### Double check for duplicates in file
``` awk '{print $1, $2}' ortho-coortho.txt | sort | uniq -cd```

### Make bed from gff3 from each sp in ortho-coortho
``` agat_convert_sp_gff2bed.pl -gff sp1.gff -o sp1.bed```

### Reduce bed to first four columns
``` cut -f1-4 sp1.bed > sp1_3col.bed```

### Give species distinction for chromosome in bed, chrm is current chromosome id, sp is two character species identifier. 
``` sed -e 's/chrm/sp/g' sp1_3col.bed > sp.bed```

### Give species distinction for gene in bed
``` sed -e 's|g|sp|g' sp.bed >ss_1.bed ```

### Give species distinction in ortho-coortho, that matches that of bed. For example
```sed -e 's/scurzebr|g/sz/g' ortho-coortho_4.txt > ortho-coortho_5.txt```

### Concatenate all bed files with sp distinction
 ``` cat sp1.bed sp2.bed sp3.bed > ortho-coortho_5.bed ```

### Rearrange columns so that they are chrm, geneID, start, end
``` awk -F'\t' '{print $1 "\t" $4 "\t" $2 "\t" $3}' ortho-coortho_5.bed > ortho-coortho_5_2.bed ```

## Run MCScan_h

### Navigate to where MCScan is installed
```cd ~/miniconda3/envs/mcscan2/MCScanX-master```

### Make working directory for run
```mkdir sp3_date ```

### Copy files to working directory, renaming extensions as per MCScan requirements
``` cp /full_path/previous_directory/ortho-coortho_5_2.bed ortho-coortho_5.gff```

``` cp /full_path/previous_directory/ortho-coortho_5.txt ortho-coortho_5.homology```

### Run MCScan_h
``` nohup ./MCScanX_h sp3_date/ortho-coortho_5```


