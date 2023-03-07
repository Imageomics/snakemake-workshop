---
title: "Reuse another workflow"
teaching: 10
exercises: 2
---

To re-use a workflow you generally need:

- where to find workflow - github "hdr-bgnn/BGNN_Core_Workflow"
- what is the relative path to the Snakefile "workflow/Snakefile"
- what tag or version to use "1.0.0"
- what file naming convention is the workflow using

https://github.com/hdr-bgnn/BGNN_Core_Workflow/blob/main/workflow/Snakefile#L19

```
module bgnn_core:
    snakefile:
        github("hdr-bgnn/BGNN_Core_Workflow", path="workflow/Snakefile", tag="1.0.0")

use rule generate_metadata from bgnn_core
```

```bash
snakemake -c1 --use-singularity DrexelMetadata/bj373514.json
```

```output
Building DAG of jobs...
MissingInputException in rule generate_metadata in file https://raw.githubusercontent.com/hdr-bgnn/BGNN_Core_Workflow/1.0.0/workflow/Snakefile, line 19:
Missing input files for rule generate_metadata:
    output: DrexelMetadata/bj373514.json, Mask/bj373514_mask.png
    wildcards: image=bj373514
    affected files:
        Images/bj373514.jpg
```

## Harmonize Filename Plans

### Option 1: Override settings in the other workflow's rules
```
use rule generate_metadata from bgnn_core with:
    input: "images/{image}.jpg"
```

### Option 2: Change our filename plan to match theirs
```
...
    return expand("Images/{ark_id}.jpg", ark_id=ark_ids)
...
rule download_image:
    input: config["filter_multimedia"]
    params: url=get_image_url
    output: "Images/{ark_id}.jpg"
    ...
```

```bash
snakemake -c1 --use-singularity DrexelMetadata/bj373514.json
```

## Bring in additional rules from BGNN_Core_Workflow
```
use rule generate_metadata from bgnn_core
use rule transform_metadata from bgnn_core
use rule crop_image from bgnn_core with:
use rule segment_image from bgnn_core
```

```bash
snakemake -c1 --use-singularity Segmented/bj373514_segmented.png 
```
