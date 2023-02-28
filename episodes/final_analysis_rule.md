---
title: "Add Summary Analysis"
teaching: 10
exercises: 2
---


```
def get_analyze_results_inputs(wildcards):
  df = pd.read_csv("filter/images.csv")
  ark_ids = df["arkID"].tolist()
  return {
     "script": "AnalyzeResults.R",
     
     "morphology": expand('Morphology/{arkID}_presence.json', arkID=ark_ids) + ["FishSummary.Rmd"]
  }

rule analyze_results:
  input: get_analyze_results_inputs
  output: "results/summary.csv"
  shell: "RScript {input.script}"
```

