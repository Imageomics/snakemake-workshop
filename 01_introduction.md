---
title: "Introduction"
teaching: 10
---

:::::::::::::::::::::::::::::::::::::: questions 

- Why not use a general purpose language like bash or python for writing workflows?
- What problems do workflow languages solve?
- What are the strengths and weaknesses of Snakemake?
- What will be covered in this workshop?

::::::::::::::::::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::: objectives

- Understand why one would use a workflow language like Snakemake
- Understand the goals of the workshop

::::::::::::::::::::::::::::::::::::::::::::::::

## Workflow Challenges
Using a general purpose language like bash or python can be challenging:

- Not re-running the whole pipeline every time
- Adapting the pipeline to run in different environments
- Providing dependencies for tools
- Tracking progress of the workflow

## Workflow language features
- Portable
- Reproducible
- Scalable
- Reusable

## Strengths of Snakemake
- Readability
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
  - Reuses an existing Snakemake workflow
