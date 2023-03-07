---
title: "Run at Scale"
teaching: 10
exercises: 2
---

## Problems with way we have been running snakemake
- Only using a single node so limited scaling
- Must keep our terminal window connected or the job might stop
- Interactive job must request maximum resources needed for all jobs: threads, cpus, gpu

## Running with Slurm
- Configure snakemake to submit slurm jobs
- Run main snakemake job in a background job
- Ensure rules request appropriate resources
  - threads/cpus
  - memory
  - requires a gpu

See [Snakemake threads/resources docs](https://snakemake.readthedocs.io/en/stable/snakefiles/rules.html#threads for details on how to request different resources such as threads, memory, and gpus.

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

## Create sbatch script
Create a script name `run-workflow.sh` to run snakemake in the background.
```
#!/bin/bash
#SBATCH --account=PAS2136
#SBATCH --time=00:30:00
. Scripts/setup_env.sh
JOBS=10
snakemake --jobs $JOBS --use-singularity --profile slurm/ "$@"
```

## Delete All Outputs
```bash
snakemake c1 --delete-all-output
```

## Run Background job and monitor progress
Run snakemake in the background scaling up
```bash
sbatch run-workflow.sh -c10
```

```
squeue -u $LOGNAME
```
