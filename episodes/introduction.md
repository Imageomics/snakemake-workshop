---
title: "Introduction"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- TODO

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- TODO

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction


Explain why one would use a workflow language when developing a workflow
Show high level view of the workflow we going building
	Show challenges when using bash or another scripting language
		not re-running the entire process every time
		scaling
Explain how Snakemake works to addresses these challenges
snakemake rule: input, output, shell
can use python in Snakefile

## Workflow Steps

### Input CSV
![multimedia CSV screenshot](files/multimedia.png)

### Step 1: Reduce the size of our input CSV
### Step 2: Filter CSV for our target species
![filtered CSV screenshot](files/filtered.png)

### Step 3: Download images
![minnow image](files/bj373514.png)

### Step 4: Detect fish in each image

### Step 5: Crop each fish image
![cropped minnow image](files/bj373514_crop.png)

### Step 6: Segment cropped images
![segmented minnow image](files/bj373514_seg.png)

### Step 7. determine presence absence morphology 
### Step 8. final analysis
![final report screenshot](files/report.png)

