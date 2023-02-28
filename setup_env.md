---
title: "Environment Setup"
teaching: 1
exercises: 4
---

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

::: callout
## New Terminal Sessions
If you close your terminal and create a new one you will need to setup your environment again.
To do so rerun the following two steps:
```bash
. setup_env.sh
sinteractive -t 01:00:00 -A PAS2136
```
:::
