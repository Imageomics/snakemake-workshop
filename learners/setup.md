---
title: Setup
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
```
Activating minconda3 environment with snakemake installed.
Activating R module that can be used with R scripts.
This module will switch Compiler environment to gnu/11.2.0 and load mkl/2021.3.0 for R/4.2.1
Configuring singularity to use a cache to speed up pulling containers.
```

Test snakemake with the --version flag
```bash
snakemake --version
```

Expected Output:
```
7.22.0
```

Test R --version flag
```bash
R --version
```

Expected Output:
```
R version 4.2.1 ...
```

## Copy lesson Files
Four input files are provided for this lesson.
An input csv file and some R scripts to filter and create the final analysis. 
These files need to be copied into the "SnakemakeWorkflow" subdirectory within your home directory.

Run the following command to copy these files into the appropriate directory:
```bash
cp /fs/ess/PAS2136/Workshops/Snakemake/files/* ~/SnakemakeWorkflow/.
```

Next run the `ls` command to ensure you have all the needed files:
```bash
ls
```
Expected Output:
```
AnalyzeResults.R  FilterImages.R  FishSummary.Rmd  multimedia.csv
```
