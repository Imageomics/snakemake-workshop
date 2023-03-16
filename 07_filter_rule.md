---
title: "Add an R Script"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can I create a rule using an RStudio friendly R script?
- How can I control what files Snakemake creates?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Create a rule that uses an RStudio friendly R script.
- Run snakemake specifying a __target output file__
- Create an __all__ rule to control the default output files
- Use a __YAML config file__ to avoid filename duplication

::::::::::::::::::::::::::::::::::::::::::::::::

## RStudio friendly Rules
Next we want to add an R script that will filter an input CSV file for the species of our choosing.
We want this R script to be easily runnable within RStudio. To accomplish this we want to avoid 
passing command line arguments to the R script as this is difficult to accommodate within RStudio.

## See the hard coded paths in the R script
```bash
head Scripts/FilterImagesHardCoded.R
```

```output
input_path <- "reduce/multimedia.csv"
output_path <- "filter/multimedia.csv"
...
```

## Run a filtering R script
Add a new rule to the bottom of `Snakefile`:
```
rule filter:
  input: 
      script="Scripts/FilterImagesHardCoded.R",
      fishes="reduce/multimedia.csv"
  output: "filter/multimedia.csv"
  shell: "Rscript {input.script}"
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
Snakemake didn't create the `filter/multimedia.csv` because the default target in snakemake is the first rule.

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

## Add a default rule
At the top of Snakefile add a new rule

```
rule all:
    input: "filter/multimedia.csv"
```

Test it out by removing filter/multimedia.csv and running snakemake with no target
```
rm filter/multimedia.csv
snakemake -c1
```

## See how the R script will reads from the config file:
```bash
head Scripts/FilterImages.R
```

## Use a config file to avoid filename duplication
A better option to allow easy changes to the rows param is to store this value in a config file.
Snakemake comes with support for parsing and using a YAML config file.

Create a new file named `config.yaml` with the following contents:
```
reduce_multimedia: reduce/multimedia.csv
filter_multimedia: filter/multimedia.csv
```

Then update your `Snakefile` to adding the config file location and using config to lookup filenames:
```
configfile: "config.yaml"

rule all:
    input: config["filter_multimedia"]

rule reduce:
  input: "multimedia.csv"
  params: rows="21"  
  output: config["reduce_multimedia"]
  shell: "head -n {params.rows} {input} > {output}"

rule filter:
  input: 
      script="Scripts/FilterImages.R",
      fishes=config["reduce_multimedia"]
  output: config["filter_multimedia"]
  shell: "Rscript {input.script}"
```

Running the workflow again:
```bash
snakemake -c1
```
