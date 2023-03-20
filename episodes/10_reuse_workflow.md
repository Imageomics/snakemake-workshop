---
title: "Reuse another workflow"
teaching: 10
exercises: 2
---
:::::::::::::::::::::::::::::::::::::: questions 

- What do I need to do to re-use another Snakemake workflow?

::::::::::::::::::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::: objectives

- Add a module to our Snakefile to re-use parts of the BGNN_Core_Workflow workflow
- Make changes so our workflow will be filename compatible

::::::::::::::::::::::::::::::::::::::::::::::::

## Reusing Another Workflow
To re-use a workflow you generally need:

- Where to find workflow? - github "hdr-bgnn/BGNN_Core_Workflow"
- What is the relative path to the Snakefile? "workflow/Snakefile"
- What tag or version to use? "1.0.0"
- What file naming convention is the workflow using?
- What dependencies must be manually installed?

See source code for the workflow
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
use rule crop_image from bgnn_core
use rule segment_image from bgnn_core
```

```bash
snakemake -c1 --use-singularity Segmented/bj373514_segmented.png 
```


