---
title: "Run at Scale"
teaching: 6
exercises: 8
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can I efficently scale up my workflow in a cluster?

::::::::::::::::::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::: objectives

- Add a memory requirement to a rule
- View a generic sbatch script to run a workflow at scale
- Run the workflow at scale

::::::::::::::::::::::::::::::::::::::::::::::::

## Running with Slurm
- Ensure rules request appropriate resources
  - threads/cpus
  - memory
  - requires a gpu
- Configure snakemake to submit slurm jobs
- Run main snakemake job in a background job

See [Snakemake threads/resources docs](https://snakemake.readthedocs.io/en/stable/snakefiles/rules.html#threads) for details on how to request different resources such as threads, memory, and gpus.

## Annotating memory requirements
Update the reduce rule to request a specific amount of memory.
```
rule reduce:
    input: "multimedia.csv"
    params: rows="11"
    output: "reduce/multimedia.csv"
    resources:
        mem_mb=200
    shell: "head -n {params.rows} {input} > {output}"
```

## Review sbatch script
The `run-workflow.sh` script was copied into your `SnakemakeWorkflow` during the project setup step.
Run the following command to view it:
```shell
cat run-workflow.sh
```

```output
#!/bin/bash
#SBATCH --account=PAS2136
#SBATCH --time=00:30:00
. Scripts/setup_env.sh
JOBS=10
snakemake --jobs $JOBS --use-singularity --profile slurm/
```

## Run Background job and monitor progress
Run snakemake in the background scaling up
```bash
sbatch run-workflow.sh
```
```output
Submitted batch job 23985835
```

## Monitor job
```
squeue -u $LOGNAME
```

```
tail -f slurm-<sbatch_job_number>.out
```

## Notice new job logs

Where did my logs go?
```bash
ls logs/
```
