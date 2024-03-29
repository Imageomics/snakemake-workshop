---
title: "Use a container"
teaching: 5
exercises: 3
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can I make my workflow more reproducible?
- How do you use a docker image with Snakemake?
- How do I enable containers with snakemake?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Use a container in a rule
- Run snakemake with the --use-singularity argument

::::::::::::::::::::::::::::::::::::::::::::::::

## Snakemake dependency management
- containers
  - exact software versions on later installs
  - support for 
    - docker container images
    - singularity container images
- conda environments
  - may result in different software versions on later installs
  - powerful but might not install in every environment

## Singularity Container Image URI

Pulling a container with docker (won't work on OSC):
```bash
docker pull alpine:3
```
```output
bash: docker: command not found
```

Pulling a docker container with singularity:
```
singularity pull docker://alpine:3
```
Singularity supports multiple container types so you must prefix docker container image URIs with `docker://`.


## Use container when downloading images
Update the download_image rule to use the docker container image URI
`quay.io/biocontainers/gnu-wget:1.18--h60da905_7`.


```
rule download_image:
    params: url=get_image_url    
    output:'Images/{ark_id}.jpg'
    container: "docker://quay.io/biocontainers/gnu-wget:1.18--h60da905_7"    
    shell: "wget -O {output} {params.url}"
```

NOTE: You must put the `container` line before the `shell` line.

Delete an image then run snakemake passing the --use-singularity flag
```bash
rm Images/hd529k3h.jpg
snakemake -c1 --use-singularity Images/hd529k3h.jpg
```

```output
Building DAG of jobs...
Pulling singularity image docker://quay.io/biocontainers/gnu-wget:1.18--h60da905_7.
Using shell: /usr/bin/bash
...
Activating singularity image /users/PAS2136/jbradley/SnakemakeWorkflow/.snakemake/singularity/3a63838ec9e2427957182dedc234c8d7.simg
...
```
