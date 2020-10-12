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
- [20201010-moa-lgbm-benchmark-v2.ipynb](notebooks/20201010-moa-lgbm-benchmark-v2.ipynb)
  - compare treat Vs.ctrl and minor modifications, StratifiedKFold
  
- [I'm interested in MLSOMOTE](kaggle_notebooks/upsampling-multilabel-data-with-mlsmote.ipynb)
  - [MLSMOTE- Approaching imbalanced multilabel learning throughsynthetic instance generation](https://www.sciencedirect.com/science/article/pii/S0950705115002737)
  - reference: https://www.kaggle.com/c/lish-moa/discussion/187419
 
- [Discovering the anticancer potential of non-oncology drugs by systematic viability profiling](https://www.nature.com/articles/s43018-019-0018-6)
- [A Next Generation Connectivity Map: L1000 Platform and the First 1,000,000 Profiles](https://www.sciencedirect.com/science/article/pii/S0092867417313090?via%3Dihub)
 
## 20201011
- [20201011-moa-lgbm-benchmark-v3.ipynb](notebooks/20201011-moa-lgbm-benchmark-v3.ipynb)
  - A Debug mode was added to increase the number of experiments.
  - CV: 0.0803, LB: 0.12627
  - [log](log/log.v3.log)
  - reference:https://zenn.dev/fkubota/articles/2b8d46b11c178ac2fa2d
  
  ![feature_importance_v3](https://user-images.githubusercontent.com/50528980/95686104-8604fe80-0bc1-11eb-876a-de8e86ab92bf.png)

- [20201011-moa-lgbm-benchmark-v4.ipynb](notebooks/20201011-moa-lgbm-benchmark-v4.ipynb)
  - Clipping a control with an outlier
  
  - Before → After
  <img width="279" alt="Screen Shot 2020-10-11 at 22 42 49" src="https://user-images.githubusercontent.com/50528980/95705834-cd6ba900-0c1a-11eb-9428-11a519709b2b.png">
  <img width="279" alt="Screen Shot 2020-10-11 at 22 42 53" src="https://user-images.githubusercontent.com/50528980/95705880-f7bd6680-0c1a-11eb-839a-86a08aadfe14.png">

