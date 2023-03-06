---
title: "Use a container"
teaching: 10
exercises: 2
---

Explain converting docker container URL to singularity container URL
Explain the need for --use-singularity flag.
Explain other options singularity image files, conda environment


## Use container when downloading images
```
rule download_image:
    input: config["filter_multimedia"]
    output:'Images/{ark_id}.jpg'
    params: url=get_image_url
    container: "docker://quay.io/biocontainers/gnu-wget:1.18--hed695b0_4"
    shell: "wget -O {output} {params.url}"
```

Pass the --use-singularity flag
```bash
snakemake -c1 --use-singularity images/hd529k3h.jpg
```
