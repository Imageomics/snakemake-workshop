---
title: "Introduction"
teaching: 10
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

## Workflow language features
- Portable
- Reproducible
- Scalable
- Reusable

## Strengths of Snakemake
- Only creates missing or out of date files
- Flexible control over which files are created 
- Uses potentially familiar Python syntax
- Dynamic branching - Output files and commands used determined while the workflow is running

## Weaknesses of Snakemake
- Rule based logic instead of procedural logic
- Requires some python code/knowledge for typical workflows

## Class Plan
- Create Snakemake Workflow that
  - Runs R scripts for filtering and final analysis
  - Runs Machine Learning Components
  - Efficiently processes many files at the same time
