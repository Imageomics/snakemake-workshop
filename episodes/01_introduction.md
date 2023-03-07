---
title: "Introduction"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- What problems do workflow languages solve?
- What are the strengths and weaknesses of Snakemake?
- What will be covered in this workshop?

::::::::::::::::::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::: objectives

- Understand why one may use a workflow language like Snakemake
- Understand the goal of the workshop

::::::::::::::::::::::::::::::::::::::::::::::::

## Why use a Workflow Language?
### Workflow Challenges
- Reproducibility
- Not re-running the entire workflow on every change
- Dependency management
- Efficiently scaling the workflow using a HPC Cluster

## Strengths and Weaknesses of Snakemake

## Strengths
- Only creates missing or out of date files
- Flexible control over which files are created 
- Uses potentially familiar Python syntax

## Weaknesses
- Rule based logic instead of procedural logic
- Requires some python code/knowledge for typical workflows

## Class Plan
- Create and run a Snakemake Workflow
  - Runs R scripts for filtering and final analysis
  - Runs Machine Learning Components
  - Process many files at the same time
  - Scale workflow on Slurm cluster


