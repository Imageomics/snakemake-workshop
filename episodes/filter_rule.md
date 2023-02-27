---
title: "Add a filtering Rule"
teaching: 10
exercises: 2
---


## RStudio friendly Rules
Next we want to add an R script that will filter an input CSV file for the species of our choosing.
We want this R script to be easily runnable within RStudio. To accomplish this we want to avoid 
passing command line arguments to the R script as this is difficult to accommodate within Rstudio.

Snakemake has built in support for a YAML config file that can be used to store configuration values
that often change.

## Create a YAML config file
Create a file named `config/config.yaml`:
```
reduced_multimedia <- "reduce/multimedia.csv"
filtered_multimedia <- "filter/multimedia.csv"
```

## Setup config.yaml in Snakefile
Add the following to the top of `Snakefile`:
```
configfile: "config/config.yaml"
```

## Create a filter rule that uses the config file:
Create a rule:
```
rule filter:
  input: 
    script="FilterImages.R",
    fishes=config["reduced_multimedia"]
  output: config["filtered_multimedia"]
  shell: "Rscript {input.script}"
```

## See how the R script will also read from the config file:
```bash
head FilterImages.R
```

```bash
snakemake -c1
```

Expected Output:
```
NOTHING happened
```

Explain All rule:
The First rule in a Snakefile is the default target.

## Run snakemake specifying a specific target
```bash
snakemake -c1 filter/multimedia.csv
```


