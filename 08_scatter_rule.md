---
title: "Process multiple files"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can I create a generic rule that can process multiple files?
- How can I use a python function as part of a rule?
- How do I tell snakemake to create a certain file before running a python function? 

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Use a python function as a params input
- Create a pattern rule to process multiple files
- Setup a __checkpoint__ to ensure a file exists before running a python function.

::::::::::::::::::::::::::::::::::::::::::::::::


## Use wget to download an image
```bash
head -n 2 filter/multimedia.csv
wget -O test.jpg https://bgnn.tulane.edu/hdr-share/ftp/ark/89609/GLIN/FMNH/bj373514.jpg
```

## Create a rule that uses a python function
Add a rule to download a single image that uses a python function param:
```
def get_image_url(wildcards):
    base_image_url = "https://bgnn.tulane.edu/hdr-share/ftp/ark/89609/GLIN/FMNH/"
    return base_image_url + "bj373514.jpg"

rule download_image:
    input: config["filter_multimedia"]
    params: url=get_image_url    
    output:"images/bj373514.jpg"
    shell: "wget -O {output} {params.url}"
```

## Make the rule generic with a pattern rule
Change this rule to be a pattern rule by adding a wildcard expression in an output filename.
```
def get_image_url(wildcards):
    base_url = "https://bgnn.tulane.edu/hdr-share/ftp/ark/89609/GLIN/FMNH/"
    return base_url + wildcards.ark_id + ".jpg"

rule download_image:
    input: config["filter_multimedia"]
    params: url=get_image_url    
    output: "images/{ark_id}.jpg"
    shell: "wget -O {output} {params.url}"
```

Run this rule:
```bash
snakemake -c1 images/hd529k3h.jpg
```

## Add python logic to lookup the URL
Add a pandas import to the top of `Snakefile`:
```
import pandas as pd
```

Change the `get_image_url` function to read the CSV file:
```
def get_image_url(wildcards):
    filename = config["filter_multimedia"]
    df = pd.read_csv(filename)
    row = df[df["arkID"] == wildcards.ark_id]
    url = row["accessURI"].item()
    return url

rule download_image:
    input: config["filter_multimedia"]
    params: url=get_image_url    
    output: "images/{ark_id}.jpg"
    shell: "wget -O {output} {params.url}"
```

Run this rule for a couple files after deleting the images directory:
```bash
rm -rf images
snakemake -c1 images/bj373514.jpg images/hd529k3h.jpg
```

## Updating all rule
```
def get_image_filenames(wildcards):
    filename = config["filter_multimedia"]
    df = pd.read_csv(filename)    
    ark_ids = df["arkID"].tolist()
    return expand("images/{ark_id}.jpg", ark_id=ark_ids)

rule all:
    input: get_image_filenames
```

```bash
snakemake -c1
```

## Test starting from scratch
```bash
rm -rf reduce filter images
snakemake -c1
```

```output
Building DAG of jobs...
FileNotFoundError in file /users/PAS2136/jbradley/SnakemakeWorkflow/Snakefile, line 7:
[Errno 2] No such file or directory: 'filter/multimedia.csv'
...
```

## Require a file exists before a python function is used
```
def get_image_filenames(wildcards):
    filename = checkpoints.filter.get().output[0]
    df = pd.read_csv(filename)    
    ark_ids = df["arkID"].tolist()
    return expand("images/{ark_id}.jpg", ark_id=ark_ids)  
...

checkpoint filter:
  input: 
      script="Scripts/FilterImages.R",
      fishes=config["reduce_multimedia"]
  output: config["filter_multimedia"]
  shell: "Rscript {input.script}"
```
