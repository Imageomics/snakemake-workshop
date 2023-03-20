---
title: "Reuse another workflow"
teaching: 7
exercises: 4
---
:::::::::::::::::::::::::::::::::::::: questions 

- What is needed to re-use another Snakemake workflow?

::::::::::::::::::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::: objectives

- Re-use parts of the BGNN_Core_Workflow Snakemake Workflow
- Make changes so our workflow will be filename compatible with another workflow

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

If there is a filename mismatch you can either change your workflow or override settings in the rule.

```
module bgnn_core:
    snakefile:
        github("hdr-bgnn/BGNN_Core_Workflow", path="workflow/Snakefile", tag="1.0.0")

use rule generate_metadata from bgnn_core
```

```bash
snakemake -c1 --use-singularity DrexelMetadata/bj373514.json
```

## Bring in additional rules from BGNN_Core_Workflow
```
use rule transform_metadata from bgnn_core
use rule crop_image from bgnn_core
use rule segment_image from bgnn_core
```

```bash
snakemake -c1 --use-singularity Segmented/hd529k3h_segmented.png
```
