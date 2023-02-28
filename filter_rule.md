---
title: "Add an R Script"
teaching: 10
exercises: 2
---


## RStudio friendly Rules
Next we want to add an R script that will filter an input CSV file for the species of our choosing.
We want this R script to be easily runnable within RStudio. To accomplish this we want to avoid 
passing command line arguments to the R script as this is difficult to accommodate within Rstudio.

### Add input and output filenames to config
Add two new lines to config.yaml with the input and output filenames.
```
reduced_multimedia: reduce/multimedia.csv
filtered_multimedia: filter/multimedia.csv
```

## Create a rule that uses named input files
Create a rule:
```
rule filter:
  input: 
    script="FilterImages.R"
    fishes=config["reduced_multimedia"]
  output: filtered_multimedia["filtered_multimedia"]
  shell: "Rscript {input.script}"
```

## See how the R script will also reads from the config file:
```bash
head FilterImages.R
```

```bash
snakemake -c1
```

Expected Output:
```output
Building DAG of jobs...
Nothing to be done (all requested files are present and up to date).
Complete log: .snakemake/log/2023-02-28T164348.500545.snakemake.log
```

## Default target
So unless you specify another target snakemake will 
The First rule in a Snakefile is the default target.

## Run snakemake specifying a specific target
By default Snakemake assumes the first rule is the default target.

For our workflow this is currently the __reduce__ rule.
To change the target file you can just pass it as an argument to snakemake.
This is a nice way to build different files from your workflow when developing it.

Run snakemake specifying `filter/multimedia.csv` as the target.
```bash
snakemake -c1 filter/multimedia.csv
```

## Run snakemake specifying a non-existant target
```bash
snakemake -c1 winninglotterynumbers.txt
```

```output
Building DAG of jobs...
MissingRuleException:
No rule to produce winninglotterynumbers.txt (if you use input functions make sure that they don't raise unexpected exceptions).
```
