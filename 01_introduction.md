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

- Understand why one would use a workflow language like Snakemake
- Understand the goals of the workshop

::::::::::::::::::::::::::::::::::::::::::::::::

## Workflow language features
- Portable
- Reproducible
- Scalable
- Reusable

## Strengths of Snakemake
- Only creates missing or out of date files
- Flexible control over which files are created 
- Is python with some additional rule syntax
- Dynamic branching
  - Workflow isn't fixed at start up. 
  - Outputs of commands can be used determined what happens next.

## Weaknesses of Snakemake
- Requires learning rule based logic instead of procedural logic
- Requires some python code/knowledge for typical workflows

## Class Plan
- Create Snakemake Workflow that
  - Runs R scripts for filtering and final analysis
  - Runs Machine Learning Components
  - Efficiently processes many files at the same time
