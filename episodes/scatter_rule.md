---
title: "Process multiple files"
teaching: 10
exercises: 2
---



First add a rule to download a single image that uses a function param:
```
rule download_image:
  input: "filter/images.csv"
  output:'images/bj373514.jpg'
  params: download_uri = "https://bgnn.tulane.edu/hdr-share/ftp/ark/89609/GLIN/FMNH/bj373514.jpg"
  shell: "wget -O {output} {params.download_uri}"
```

Run this rule:
```bash
snakemake -c1 images/bj373514.jpg
```

Change this rule to be a pattern rule:
```
rule download_image:
  input: "filter/images.csv"
  output:'images/{arkID}.jpg'
  params: download_uri="https://bgnn.tulane.edu/hdr-share/ftp/ark/89609/GLIN/FMNH/{arkID}.jpg"
  shell: "wget -O {output} {params.download_uri}"
```

Run this rule:
```bash
snakemake -c1 images/bj373514.jpg
```

Make rule handle all our images with a function based param.
Add import to the top of `Snakefile`:
```
import pandas as pd
```

Replace the `download_image` rule with the following:
```
def download_image_url(wildcards):
  df = pd.read_csv("filter/images.csv")
  row = df[df["arkID"] == wildcards.ark_id]
  url = row["accessURI"].item()
  return url

rule download_image:
  input: "filter/images.csv"
  output:'images/{ark_id}.jpg'
  params: download_uri=download_image_url
  shell: "wget -O {output} {params.download_uri}"
```

Run this rule for a couple files:
```bash
snakemake -c1 images/bj373514.jpg images/hd529k3h.jpg
```

TODO: Decide if we should create all rule here:
```
def get_workflow_targets():
    df = pd.read_csv("filter/images.csv")
    ark_ids = df["arkID"].tolist()
    return expand("images/{ark_id}.jpg", ark_id=ark_ids)

rule all:
   input: get_workflow_targets
```

