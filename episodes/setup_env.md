---
title: "Project Setup"
teaching: 10
exercises: 2
---

## Log into OSC
- Visit https://ondemand.osc.edu/.
- Login with your credentials.
- From the top menu select __Files__ -> __Home Directory__
- Click the __New Directory__ button
  - Enter `SnakemakeWorkflow` for the directory name
- Click "SnakemakeWorkflow" in the __Name__ column the list of files and folders
- Click "open in terminal"

## Software Environment Setup

Run the following steps from within an OSC ondemand terminal within the "SnakemakeWorkflow" subdirectory.

Activate the Snakemake environment
```bash
. /fs/ess/PAS2136/Workshops/Snakemake/setup_env.sh
```

Expected Output:
```output
Activating R module that can be used with R scripts.
This module will switch Compiler environment to gnu/11.2.0 and load mkl/2021.3.0 for R/4.2.1
Lmod is automatically replacing "intel/19.0.5" with "gnu/11.2.0".
The following have been reloaded with a version change:
  1) mvapich2/2.3.3 => mvapich2/2.3.6
Configuring R to use the yaml package.
Configuring singularity to use a cache to speed up pulling containers.
```

Test snakemake with the --version flag
```bash
snakemake --version
```

Expected Output:
```output
7.22.0
```

Test R --version flag
```bash
R --version
```

Expected Output:
```output
R version 4.2.1 ...
```

## Copy lesson Files
Four input files are provided for this lesson.
An input csv file and some R scripts to filter and create the final analysis. 
These files need to be copied into the "SnakemakeWorkflow" subdirectory within your home directory.

Run the following command to copy these files into your new SnakemakeWorkflow directory:
```bash
cp -r /fs/ess/PAS2136/Workshops/Snakemake/files/* ~/SnakemakeWorkflow/.
```

Next run the `ls` command to ensure you have all the needed files:
```bash
ls
```
Expected Output:
```output
AnalyzeResults.R  FilterImages.R  FishSummary.Rmd  multimedia.csv
```

## Start Interactive Session
Start an interactive session that will remain active for 1 hour of by running:
```bash
sinteractive -t 01:00:00 -A PAS2136
```

