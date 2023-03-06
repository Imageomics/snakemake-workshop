---
title: "Introduction"
teaching: 10
exercises: 2
---

## Goals
- Understanding the problem that workflow languages are trying to solve
- Understand the strengths and weaknesses of the Snakemake workflow language
- Create and run a Snakemake Workflow
  - Runs R scripts for filtering and final analysis
  - Runs Machine Learning Components
  - Process many files at the same time

## Why use a Workflow Language?
### Workflow Challenges
- Reproducibility
- Not re-running the entire workflow on every change
- Dependency management
- Efficiently scaling the workflow using a HPC Cluster



## Strengths and Weaknesses of Snakemake

## Strengths
- Rerun only creates missing or out of date files
- Python syntax

## Weaknesses
- Rule based logic instead of procedural logic
- Requires some python code/knowledge for typical workflows


## How Snakemake works

### Snakemake
- Breaks up workflow into rules and runs only the parts necessary
- Provides support for specifying containers for each rule
- Provides support for many HPC Clusters including SLURM

Explain DAG

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


