---
title: "Use a container"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can I specify a container for a rule?
- How do you change a docker image URI into a singularity image URI?
- What command line argument is required to enable containers for snakemake ? 

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
    - singularity containers images
- conda environments
  - may result in different software versions on later installs
  - powerful but might not install in every environment

## Singularity Container Image URI

Pulling a container with docker:
```
docker pull ubuntu:kinetic-20230217
```

Pulling a docker container with singularity:
```
singularity pull docker://ubuntu:kinetic-20230217
```

## Use container when downloading images

```
rule download_image:
    input: config["filter_multimedia"]
    params: url=get_image_url    
    output:'Images/{ark_id}.jpg'
    container: "docker://quay.io/biocontainers/gnu-wget:1.18--h60da905_7"    
    shell: "wget -O {output} {params.url}"
```



Delete an image then run snakemake passing the --use-singularity flag
```bash
rm images/hd529k3h.jpg
snakemake -c1 --use-singularity images/hd529k3h.jpg
```

```output
Building DAG of jobs...
Pulling singularity image docker://quay.io/biocontainers/gnu-wget:1.18--h60da905_7.
Using shell: /usr/bin/bash
...
Activating singularity image /users/PAS2136/jbradley/SnakemakeWorkflow/.snakemake/singularity/3a63838ec9e2427957182dedc234c8d7.simg
...
```
