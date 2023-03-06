---
title: "Reuse another workflow"
teaching: 10
exercises: 2
---

To re-use a workflow you generally need:

- where to find workflow - github "hdr-bgnn/BGNN_Core_Workflow"
- what is the relative path to the Snakefile "workflow/Snakefile"
- what tag or version to use "1.0.0"

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
