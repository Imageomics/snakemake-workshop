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
snakemake -c1 detect/metdata_bj373514.json
```

Expected output: Fails to find command :(

Pass the --use-singularity flag
```
snakemake -c1 detect/metdata_bj373514.json --use-singularity
```
