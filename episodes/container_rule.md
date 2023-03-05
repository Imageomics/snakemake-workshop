---
title: "Use a container"
teaching: 10
exercises: 2
---


```
rule detect:
  input: 'images/{arkID}.jpg'
  output: metadata='detect/metadata_{arkID}.json',
  container: "docker://ghcr.io/hdr-bgnn/drexel_metadata:0.5"
  shell: "gen_metadata.py --device cpu --outfname {output.metadata} {input}"
```

```bash
snakemake -c1 detect/metadata_bj373514.json
```
```output
...
/usr/bin/bash: gen_metadata.py: command not found
...
```

Pass the --use-singularity flag
```
snakemake -c1 --use-singularity detect/metadata_bj373514.json 
```



```
rule create_morphological_analysis:
  input:
    image = 'segmented/{arkID}_segmented.png',
    metadata = 'transform_metadata/simple_metadata_{arkID}.json'
  output: 'Morphology/{arkID}_presence.json'
  container: "docker://ghcr.io/hdr-bgnn/morphology-analysis/morphology:1.0.0"
  shell: 'Morphology_main.py {input.image} --metadata {input.metadata} {output}'
```
