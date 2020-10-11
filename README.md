# kaggle_MoA

<img width="997" alt="Screen Shot 2020-10-08 at 14 44 57" src="https://user-images.githubusercontent.com/50528980/95506038-eeab6b80-0974-11eb-9a91-29613411768e.png">

https://www.kaggle.com/c/lish-moa

Evaluation: log loss

In this competition, we have a dataset that combines gene expression and cell viability data. 
The data were measured simultaneously human cells’ responses to drugs in a pool of 100 different cell types.
Our aim is to solve the issue of identifying ex-ante, which cell types are better suited for a given drug.in this dataset.

## 20201008
- [download benchmark notebooks](/kaggle_notebooks/)

## 20201009
- EDA
  - g- signify gene expression.
  - c- signify cell viability. 
  - cp_type indicates samples treated with a compound (trt_cp) or with a control perturbation (ctl_vehicle); control perturbations have no MoAs.
  - cp_time indicates treatment duration (24, 48, 72 hours) 
  - cp_dose indicate dose (high or low).
- [20201009_MoA: LGBM Benchmark.v1.ipynb](notebooks/20201009-moa-lgbm-benchmark-v1.ipynb)
  - starter model
  - reference: https://www.kaggle.com/sishihara/moa-lgbm-benchmark#Preprocessing

## 20201010
- [I'd like to try MLSOMOTE](kaggle_notebooks/upsampling-multilabel-data-with-mlsmote.ipynb)
  - [MLSMOTE- Approaching imbalanced multilabel learning throughsynthetic instance generation.pdf](articles/MLSMOTE- Approaching imbalanced multilabel learning throughsynthetic instance generation.pdf)
  - reference: https://www.kaggle.com/c/lish-moa/discussion/187419
  
