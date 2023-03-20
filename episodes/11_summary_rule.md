---
title: "Add Summary Analysis"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can we specify a rule that has many dynamic input files?

::::::::::::::::::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::: objectives

- Add a summary rule that requires Segmented images
- Use expand function to simplify creating filenames

::::::::::::::::::::::::::::::::::::::::::::::::


Add summary_report to config.yaml:
```bash
cat Scripts/SummaryReport.R 
```

```output
config <- yaml::read_yaml(file = "config.yaml")
filtered_images_path <- config$filter_multimedia
output_path <- config$summary_report

filtered_images <- read.csv(file = filtered_images_path)

dir.create(dirname(output_path), showWarnings = FALSE)
rmarkdown::render("Scripts/Summary.Rmd", output_file=basename(output_path), output_dir=dirname(output_path))
```

Edit config.yaml adding __summary_report__:
```
summary_report: summary/report.html
```

Change the `all` rule to require `summary/report.html` in `Snakefile`:
```
rule all:
    inputs: config["summary_report"]
```

Add a new function that gathers the inputs for the summary rule and a `summary` rule:
```
def get_summary_inputs(wildcards):
  filename = checkpoints.filter.get().output[0]
  df = pd.read_csv(filename)
  ark_ids = df["arkID"].tolist()
  return expand('Segmented/{arkID}_segmented.png', arkID=ark_ids)

rule summary:
  input:
     script="Scripts/SummaryReport.R",
     markdown="Scripts/Summary.Rmd",
     morphology=get_summary_inputs
  output: config["summary_report"]
  container: "docker://ghcr.io/rocker-org/tidyverse:4.2.2"
  shell: "Rscript {input.script}"
```

Run snakemake to create the summary/report.
```bash
snakemake -c1 --use-singularity --dry-run
```
