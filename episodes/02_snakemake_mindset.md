---
title: "Snakemake Mindset"
teaching: 10
---

:::::::::::::::::::::::::::::::::::::: questions 

- What are the parts of a snakemake workflow?
- What is a snakemake rule?
- How does snakemake combine rules into jobs to run?
- How is a Snakemake Rule different from a Snakemake Job?

::::::::::::::::::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::: objectives

- Understand a Snakemake workflow is a list of rules
- Understand the parts of a Snakemake rule
- Understand how Snakemake creates a job DAG

::::::::::::::::::::::::::::::::::::::::::::::::

## How Snakemake works

### Snakemake
- Breaks up workflow into rules and runs only the parts necessary
- Provides support for specifying containers for each rule
- Provides support for many HPC Clusters including SLURM

## What is a Snakemake Rule?

Main parts of a Snakemake Rule used in this workshop

- __name__ - unique name for a rule
- __input__ - input filenames used by a command
- __params__ - input parameter (non-file) values used by a command
- __output__ - output filenames created by a command
- __shell__ - command to run
- __container__ - singularity container to run command within

Rule pattern:

```
rule <name>:
    input: ...
    params: ...
    output: ...
    shell: ...
    container: ...
```

## How Snakemake combines rules
### Example workflow

Create checksums for each image and zip up the results
```
fish1.jpg
fish2.jpg
```
rule checksum1:
    input: "fish1.jpg"
    output: "fish1.checksum"
    ...
   
rule checksum2:
    input: "fish2.jpg"
    output: "fish2.checksum"   
    ...
   
rule zip:
    input: "fish1.jpg", "fish1.checksum", "fish2.jpg", "fish2.checksum"
    ouput: "result.zip"
    ...
```

Explain DAG
DAG (directed acyclic graph) 

Rules vs Jobs
Job is running a rule to create a specific set of output files.
