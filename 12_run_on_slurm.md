---
title: "Run at Scale"
teaching: 10
exercises: 2
---

## Scalability Issues
- Only using a single node so limited scaling
- Must keep our terminal window connected or the job might stop

## Solution
- Configure Snakemake to spawn Slurm Jobs that run in parallel
- Run whole job in the background

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
