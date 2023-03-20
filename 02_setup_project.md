---
title: "Project Setup"
teaching: 1
exercises: 5
---
:::::::::::::::::::::::::::::::::::::: questions 

- How do I create a directory with the necessary files for this class?

::::::::::::::::::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::: objectives

- Connect to OSC
- Create a directory
- Open a Web Terminal in that Directory
- Copy lesson files into place
- Copy cached singularity images into place

::::::::::::::::::::::::::::::::::::::::::::::::


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
multimedia.csv  Scripts  slurm
```

### Lesson Files
- __multimedia.csv__ - Main input file of fish images used by the workflow
- __Scripts/__
  - __setup_env.sh__ - Used to activates snakemake conda environment and other utilities
  - __FilterImagesHardCoded.R__ - Rscript that filters a CSV for a target species with hard coded filenames
  - __FilterImages.R__ - R script that filters a CSV for a target species
  - __SummaryReport.R__ - R script that builds a summary report of the workflow outputs
  - __Summary.Rmd__ - R markdown script used by SummaryReport.R to create a report
- __slurm/__ - Directory containing a config file used by Snakemake to run SLURM jobs

## Setup Snakemake Singularity Cache
To avoid waiting for singularity containers to pull we will copy cached containers inline.
```
mkdir -p .snakemake/singularity
cp /fs/ess/PAS2136/Workshops/Snakemake/singularity_images/* .snakemake/singularity/.
```
