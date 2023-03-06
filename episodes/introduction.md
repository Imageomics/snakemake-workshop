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

A Snakemake Rule specifies

- name - unique name for the rule
- input files
- output files
- shell command that reads the input file and writes the output files


Example rules
```
# Count words in one of the books
rule count_words:
    input: 'books/isles.txt'
    output: 'isles.dat'
    shell: 'python wordcount.py books/isles.txt isles.dat'
```
_This rule was copied from the [carpentries incubator hpc-workflows lesson](https://carpentries-incubator.github.io/hpc-workflows/02-snakefiles/index.html)._


## Procedural Approach

## How Snakemake combines rules


