---
title: "Creating a Snakefile"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- How do you create a Snakemake Workflow?
- How do you run a Snakemake Workflow?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain the purpose of the the input, output, and shell parts of a Snakemake Rule
- Demonstrate how to create a Snakemake Rule
::::::::::::::::::::::::::::::::::::::::::::::::



## Create a Snakemake Workflow
For our first step we will reduce the size of the __multimedia.csv__ input file
saving the result as __reduce/multimedia.csv__. This CSV file contains a header line followed by
data lines. So to save the first 20 data lines we will need the first 21 lines in total.
This can be accomplished with the shell `head` command passing the `--n 21` argument.
Run the following command in your terminal to see the first 21 lines printed out.
```bash
head -n 21 multimedia.csv
```
To save this output we will append ` > reduce/multimedia.csv` to the command like so:

`head -n 21 multimedia.csv > reduce/multimedia.csv`

Create a text file named `Snakefile` with the following contents:
```
rule reduce:
    input: "multimedia.csv"
    output: "reduce/multimedia.csv"
    shell: "head -n 21 multimedia.csv > reduce/multimedia.csv"
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
```
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
```
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
    shell: "head -n 21 {input} > {output}"
```

## Use a param to simplify future changes
Currently the reduce rule grabs the top 21 rows. Since we are likely to change this number in the future
a better option is to use a param. 

Add a new `params` setting to the `rule` and use the `params.rows` wildcard in the shell as follows:
```
rule reduce:
  input: "multimedia.csv"
  output: "reduce/multimedia.csv"
  params: rows="21"
  shell: "head -n {params.rows} {input} > {output}"
```

Try running the workflow again:
```bash
snakemake -c1
```
