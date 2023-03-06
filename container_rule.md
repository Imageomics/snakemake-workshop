---
title: "Use a container"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can I specify a singularity container for a rule?
- How do you change a docker container URI into a singularity container URI?
- What command line argument is required to enable containers for snakemake ? 

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Update a rule to use a container
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

Example docker container:
`docker pull ubuntu:kinetic-20230217`

Example docker container:
`singularity pull docker://ubuntu:kinetic-20230217`


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


Explain the need for --use-singularity flag.


