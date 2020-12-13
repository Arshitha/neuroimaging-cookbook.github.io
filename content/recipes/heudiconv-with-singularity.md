---
title: HeuDiConv with Singularity 
tags: [bids]
utility: [dcm2niix conversion]
modality: []
language: [bash]
software: [heudiconv]
---

Explain briefly what the recipe does.

- Explain briefly how the recipe works.
- Provide a citation and link to documentation of any third party software.
- Provide the version of the software/package
- Use bullet points for your recipe's explanation.
- Try to explain everything briefly but clearly.


## Ingredients: 

- Singularity v2.4 [Installation/Download](https://singularity.lbl.gov/install-mac) 
<!-- - Sample Dataset (if you) [Example](https://figshare.com/articles/MyConnectome_BIDS_tutorial2A/6267503) -->

## Method:
1. building heudiconv singularity container 

```bash
singularity build ~/singularity-containers/heudiconv-0.8.0.simg docker://nipy/heudiconv:0.8.0
```

2. downloading sample dataset/image of directory tree 

3. setting environment variables 
```bash
export SINGULARITY_BINDPATH=/Users
```
Binds your `/home` and `/data` directory to your singularity container

4. running step 1 of the heudiconv pipeline
```bash
singularity run --cleanenv \
-B /home/user/dicoms:/base \
-B /data/user/bids:/output \
<path-to-heudiconv-image> \
-d /base/{subject}/{session}/mr_001/*.dcm \
-s 001
--ses 01
-f convertall
-c none 
-o /output
```

5. editing `heuristic.py` - varies depending on scan aquistions - examples and uses cases should reflect that (most important and most confusing step) 
6. running the final step of the process 
```bash
singularity run \
-B /home/user/dicoms:/base \
-B /data/user/bids:/output \
<path-to-heudiconv-image> \
-d /base/{subject}/{session}/mr_001/*.dcm \
-s 001
--ses 01
-f <path-to-heuristic.py>
-c dcm2niix -b 
-o /output 
```
7. verifying the output directory would be a valid BIDS directory
```bash
singularity build ~/singularity-containers/bids-validator-1.5.7.simg docker://bids/validator:v1.5.7
```


## References: 
1. feingold, franklin: [Walkthrough Tutorial](https://reproducibility.stanford.edu/bids-tutorial-series-part-2a/)
2. feingold, franklin (2018): MyConnectome_BIDS_tutorial2A. figshare. Dataset. https://doi.org/10.6084/m9.figshare.6267503.v1 
3. Kent, James: [Heudiconv Tutorial](https://slides.com/jameskent/deck-3#/9)


