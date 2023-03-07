---
title: "Add Summary Analysis"
teaching: 10
exercises: 2
---

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


Where did my logs go?
```bash
ls logs
ls logs/
```


