---
title: "Create a Workflow"
teaching: 10
exercises: 6
---

:::::::::::::::::::::::::::::::::::::: questions 

- How do you create a Snakemake Workflow?
- How do you run a Snakemake Workflow?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Create a workflow with a single rule
- Run a snakemake workflow
- Understand snakemake output
- Reduce filename duplication using __wildcards__
- Represent non-file inputs using a __param__

::::::::::::::::::::::::::::::::::::::::::::::::

## Snakemake Workflow
A Snakemake workflow is a list of rules.

Commonly used parts of a rule are:

- __name__ - unique name for a rule
- __input__ - input filenames used by a command
- __params__ - non-file input values used by a command
- __output__ - output filenames created by a command
- __container__ - singularity container to run command within
- __shell__ - command to run


Rule pattern:

```
rule <name>:
    input: ...
    params: ...
    output: ...
    container: ...
    shell: ...

```

## Create a Snakemake Workflow
For our first step we will reduce the size of the __multimedia.csv__ input file
saving the result as __reduce/multimedia.csv__. This CSV file contains a header line followed by
data lines. So to save the first 10 data lines we will need the first 11 lines in total.
This can be accomplished with the shell `head` command passing the `--n 11` argument.
Run the following command in your terminal to see the first 11 lines printed out.
```bash
head -n 11 multimedia.csv
```
To save this output we will change the command like so:

`head -n 11 multimedia.csv > reduce/multimedia.csv`

Create a text file named `Snakefile` with the following contents:
```
rule reduce:
    input: "multimedia.csv"
    output: "reduce/multimedia.csv"
    shell: "head -n 11 multimedia.csv > reduce/multimedia.csv"
```
This code creates a Snakemake rule named __reduce__ with a input file __multimedia.csv__, a output file __reduce/multimedia.csv__, and the shell command from above.
Snakemake will automatically create the reduce directory for us before running the shell command.
A common pattern is to output files in a directory named after the rule.
This will help you keep track of which rule created a specific file.

## Run the Workflow

```bash
snakemake
```

Expected Error Output:
```output
Error: you need to specify the maximum number of CPU cores 
to be used at the same time. If you want to use N cores, 
say --cores N or -cN. For all cores on your system 
(be sure that this is appropriate) use --cores all. 
For no parallelization use --cores 1 or -c1....
```

The `snakemake` command has a single required argument that specifies how manu CPU cores to use when running a workflow.
Until we want to scale up our workflow we are fine using 1 core.

Run snakemake specifying 1 core:
```bash
snakemake -c1
```

Expected Output:
```output
Building DAG of jobs...
Using shell: /usr/bin/bash
Provided cores: 1 (use --cores to define parallelism)
Rules claiming more threads will be scaled down.
Job stats:
job       count    min threads    max threads
------  -------  -------------  -------------
reduce        1              1              1
total         1              1              1
Select jobs to execute...
[Mon Feb 27 11:28:29 2023]
rule reduce:
    input: multimedia.csv
    output: reduce/multimedia.csv
    jobid: 0
    reason: Missing output files: reduce/multimedia.csv
    resources: tmpdir=/tmp
[Mon Feb 27 11:28:29 2023]
Finished job 0.
1 of 1 steps (100%) done
Complete log: .snakemake/log/2023-02-27T112816.505955.snakemake.log
```

Notice the __reason__ logging `Missing output files: reduce/multimedia.csv`.
This tells you why snakemake is running this rule.
In this case it is because the output file reduce/multimedia.csv is missing.

Try running the workflow again:
```bash
snakemake -c1
```

Expected Output:
```
Building DAG of jobs...
Nothing to be done (all requested files are present and up to date).
Complete log: .snakemake/log/2023-02-27T113325.906186.snakemake.log
```

## Reduce duplication using wildcards

Within the `shell` command the __input__ and __output__ files can be referenced using wildcards.
This will reduce duplication simplifing filename changes.

Change the `shell` line in the `Snakefile` as follows:
```
rule reduce:
    input: "multimedia.csv"
    output: "reduce/multimedia.csv"
    shell: "head -n 11 {input} > {output}"
```

## Represent non-file inputs using a param
Currently the reduce rule grabs the top 11 rows. Since the meaning of this number may not be obvious and we are likely to change this number in the future a better option is to use this non-file input as a param.

Add a new `params` setting to the `rule` and use the `params.rows` wildcard in the shell as follows:
```
rule reduce:
  input: "multimedia.csv"
  params: rows="11"  
  output: "reduce/multimedia.csv"
  shell: "head -n {params.rows} {input} > {output}"
```
Notice how we assigned a name "rows" to the parameter. Assigning a name like this can also be done for the input and output filenames as well.

Try running the workflow again:
```bash
snakemake -c1
```

Notice the __reason__ has changed 
```output
...
    reason: Code has changed since last execution; Params have changed since last execution
...
```
