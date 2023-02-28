---
title: "Reuse another workflow"
teaching: 10
exercises: 2
---

## Replace object detection with the object detection rule from the BGNN_Core_Workflow

```
module segmentation:
    snakefile:
        github("hdr-bgnn/BGNN_Core_Workflow",   path="workflow/Snakefile", tag="1.0.0")
use rule generate_metadata from segmentation as object_detection with:
    input:'images/{image}.jpg'
```

```bash
snakemake -c1 --use-singularity DrexelMetadata/bj373514.json
```


## Bring in additional rules from BGNN_Core_Workflow
```
use rule generate_metadata from segmentation
use rule transform_metadata from segmentation
use rule crop_image from segmentation
use rule segment_image from segmentation
```

```bash
snakemake -c1 --use-singularity Segmented/bj373514_segmented.png 
```


