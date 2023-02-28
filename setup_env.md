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
- Click "Open in Terminal"

## Copy Lesson Files
Some files are provided for this lesson.
These files need to be copied into the "SnakemakeWorkflow" subdirectory within your home directory.
You should be within the `SnakemakeWorkflow` directory before running this step.

### Verify Directory
Run the following command to ensure you are in the appropriate directory:
```bash
pwd
```
The output should look similar to the following:
```output
/users/PAS2136/jbradley/SnakemakeWorkflow
```

### Copy Files
Run the following command to copy these files into your current directory (SnakemakeWorkflow):
```bash
cp -r /fs/ess/PAS2136/Workshops/Snakemake/files/* .
```

Next run the `ls` command to ensure you have all the needed files:
```bash
ls
```
Expected Output:
```output
AnalyzeResults.R  FilterImages.R  FishSummary.Rmd  multimedia.csv  setup_env.sh  slurm
```

### Lesson Files
- __setup_env.sh__ - Used to activates snakemake conda environment and other utilities
- __multimedia.csv__ - Main input file of fish images used by the workflow
- __FilterImages.R__ - R script that filters a CSV for a target species.
- __AnalyzeResults.R__ - R script that builds a summary report of the workflow outputs
- __FishSummary.Rmd__ - R markdown script used by __AnalyzeResults.R to create a report
- __slurm__ - Directory containing a config file used by Snakemake to run SLURM jobs

## Software Environment Setup

Run the following steps from within an OSC ondemand terminal within the "SnakemakeWorkflow" subdirectory.

Activate the Snakemake environment:
```bash
. setup_env.sh
```
NOTE: The leading dot followed by a space is important. This causes the settings in setup_env.sh to be applied
to your current terminal session.


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


## Start Interactive Session
Since we will be running some heavy processing starting an interactive job is important.
Otherwise this processing would occcur on a login node and cause issues for other cluster users.

Start an interactive session that will remain active for 1 hour of by running:
```bash
sinteractive -t 01:00:00 -A PAS2136
```

## New Terminal Sessions
If you close your terminal and create a new one you will need to setup your environment again.
To do so rerun the following two steps:
```
. setup_env.sh
sinteractive -t 01:00:00 -A PAS2136
```
