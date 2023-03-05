---
title: "Add Summary Analysis"
teaching: 10
exercises: 2
---

Add summary_report to config.yaml:

```
summary_report: summary/report.html
```

```
def get_summary_inputs(wildcards):
  filename = checkpoints.filter.get().output[0]
  df = pd.read_csv(filename)
  ark_ids = df["arkID"].tolist()
  return expand('Morphology/{arkID}_presence.json', arkID=ark_ids)

rule summary:
  input:
     script="Scripts/SummaryReport.R",
     morphology=get_summary_inputs
  output: config["summary_report"]
  shell: "Rscript {input.script}"
```

```
def get_summary_inputs(wildcards):
  filename = checkpoints.filter.get().output[0]
  df = pd.read_csv(filename)
  ark_ids = df["arkID"].tolist()
  return expand('Morphology/{arkID}_presence.json', arkID=ark_ids)

rule summary:
  input:
     script="Scripts/SummaryReport.R",
     markdown="Scripts/FishSummary.R",
     morphology=get_summary_inputs
  output: config["summary_report"]
  shell: "Rscript {input.script}"
```
