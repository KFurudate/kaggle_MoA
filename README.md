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
  - CV: 0.0803, LB: 0.12627 (Debug = True)
  - [log](log/log.v3.log)
  - reference:https://zenn.dev/fkubota/articles/2b8d46b11c178ac2fa2d
  
  ![feature_importance_v3](https://user-images.githubusercontent.com/50528980/95686104-8604fe80-0bc1-11eb-876a-de8e86ab92bf.png)

- [20201011-moa-lgbm-benchmark-v4.ipynb](notebooks/20201011-moa-lgbm-benchmark-v4.ipynb)
  - Clipping a control with an outlier(25-75)
  
  - CV: 0.03667, LB: 0.78780 (Debug = True)
  - [log](log/log.v4.log)
    
  - Before → After
  <img width="279" alt="Screen Shot 2020-10-11 at 22 42 49" src="https://user-images.githubusercontent.com/50528980/95705834-cd6ba900-0c1a-11eb-9428-11a519709b2b.png">
  <img width="279" alt="Screen Shot 2020-10-11 at 22 42 53" src="https://user-images.githubusercontent.com/50528980/95705880-f7bd6680-0c1a-11eb-839a-86a08aadfe14.png">
  
  ![feature_importance_v4](https://user-images.githubusercontent.com/50528980/95753394-2eb96980-0c67-11eb-80d2-b461401eed00.png)

## 20201012
- [20201011-moa-lgbm-benchmark-v5.ipynb](notebooks/20201011-moa-lgbm-benchmark-v5.ipynb)
  - Clipping a control with an outlier(20-80)
  - CV:0.0412, LB: 0.63746 (Debug = True)
  - [log](log/log.v5.log)
  
- [20201012drugs-classification-mechanisms-of-action.ipynb](notebooks/20201012drugs-classification-mechanisms-of-action.ipynb)
  - EDA
  - First, I want to use a highly correlated target.
  <img width="341" alt="Screen Shot 2020-10-12 at 21 14 35" src="https://user-images.githubusercontent.com/50528980/95807922-f0f12b00-0cd0-11eb-87bf-6d8bcc874f10.png">

  - Second, I want to use target correlation without score for learning.
  <img width="433" alt="Screen Shot 2020-10-12 at 21 15 56" src="https://user-images.githubusercontent.com/50528980/95807874-d454f300-0cd0-11eb-932b-7f6fbbfdab9a.png">
  
  - reference https://www.kaggle.com/amiiiney/drugs-classification-mechanisms-of-action

## 20201015
- [20201015-moa-lgbm-benchmark-v6.ipynb](notebooks/20201015-moa-lgbm-benchmark-v6.ipynb)
  - under sampling 500 → oversamplling 500, lipping a control with an outlier(10-90)
  - CV:0.04924, LB: 0.92860 (Debug = True)
  - [log](log/log.v6.log)

- UMAP control data (upper: gene expression, lower: cell viability)

  <img width="346" alt="Screen Shot 2020-10-15 at 15 32 14" src="https://user-images.githubusercontent.com/50528980/96271245-44de6700-0f92-11eb-9899-90125db174d8.png">

- UMAP train data (upper: gene expression, lower: cell viability)

  <img width="331" alt="Screen Shot 2020-10-15 at 15 32 03" src="https://user-images.githubusercontent.com/50528980/96271380-6d666100-0f92-11eb-822b-e11311deb6a7.png">

  - Clustering with data-driven is going to be difficult to do.

## 20201016
- It is not clear whether this makes sense or not. [I decided to find drugs in non scored data that belong to the same category as the data with a low number of targets in the scored data.](data/20201016_moa_sig_list.csv)
  - reference: [薬理学電子教科書](https://drugacademy.atlassian.net/wiki/spaces/PHARMACOLO/overview?mode=global)

  <img width="935" alt="Screen Shot 2020-10-16 at 21 01 12" src="https://user-images.githubusercontent.com/50528980/96326019-ccf45900-0ff2-11eb-8f2f-a511cb0c6598.png">
  
- [I want to use Pseudo Labeling](kaggle_notebooks/pseudo-labeling-qda-0-969.ipynb)
  - reference:https://www.kaggle.com/cdeotte/pseudo-labeling-qda-0-969 
  - reference: https://upura.hatenablog.com/entry/2020/02/18/180500#f-891859c5

## 20201017
- [20201016-moa-lgbm-benchmark-v7.ipynb](notebooks/20201016-moa-lgbm-benchmark-v7.ipynb)
  - [Use anotated data.](data/20201016_moa_sig_list.csv)
  - CV:0.1286671, LB:0.15010 (Debug = True)
  - [log](log/log.v7.log)
  - [feature_importance](data/feature_importance_df.v7.csv)
  
  ![feature_importance_v7](https://user-images.githubusercontent.com/50528980/96374678-a2f97e80-1139-11eb-9fe4-07522d2696b9.png)

- [20201017-moa-lgbm-benchmark-v7.ipynb](notebooks/20201017-moa-lgbm-benchmark-v7.ipynb)
  - I was not able to make it time within 9 hours in Kaggle's notebook, so I'll experiment with "Debug = False" in the local.
  
## 20201019
- [20201017-moa-lgbm-benchmark-v8.ipynb](notebooks/20201017-moa-lgbm-benchmark-v8.ipynb)
  - pasude labeling (thresholds: 0.5)
  - CV:0.0636624, LB:Notebook Timeout (Debug = True)
  - [log](log/log.v8.log)
  - [feature_importance](data/feature_importance_df.v8.csv)
  
  ![feature_importance_v8](https://user-images.githubusercontent.com/50528980/96465999-422c7d80-11ef-11eb-9dc6-19ff7369dde9.png)

## 20201020
- cell viability
  - Cell viability is a measure of the proportion of live, healthy cells within a population. 
  - To measure cell survival following treatment with compounds, such as during a drug screen.
  - reference: https://en.cellsignal.jp/contents/_/synopsis-of-cell-proliferation-metabolic-status-and-cell-death/cell-viability-and-survival

## 20201021
- [20201020-moa-lgbm-benchmark-v10.ipynb](notebooks/20201020-moa-lgbm-benchmark-v10.ipynb)
  - Pasude labeling (thresholds:0.6), ReduceCol: Kolmogorov-Smirnov, PCA(whiten)&UMAP
  - CV:0.07143886105, LB:0.08165 (Debug = True)
  - [log](log/log.v10.log)
  - [feature_importance](data/feature_importance_df.v10.csv)

  ![feature_importance_v10](https://user-images.githubusercontent.com/50528980/96805736-ecff9000-13d7-11eb-86f7-d01bc74d2c0e.png)

## 20201022
- [20201021-moa-lgbm-benchmark-v11.ipynb](notebooks/20201021-moa-lgbm-benchmark-v11.ipynb)
  - Pasude labeling (thresholds:0.6), ReduceCol: Kolmogorov-Smirnov, PCA(whiten)&UMAP, lgbm parames adjust
  - CV:0.06696406, LB:0.07817 (Debug = True)
  - [log](log/log.v11.log)
  - [feature_importance](data/feature_importance_df.v11.csv)
  
  ![feature_importance_v11](https://user-images.githubusercontent.com/50528980/96902192-d437be80-1459-11eb-8e46-4a89c78160f8.png)

- [20201022-moa-lgbm-benchmark-v8.colab.ipynb](notebooks/20201022-moa-lgbm-benchmark-v8.colab.ipynb)
  - Pasude labeling (thresholds:0.6), ReduceCol: Kolmogorov-Smirnov, PCA(whiten)&UMAP, lgbm parames adjust
  - CV:0.05454435094, LB: None(colab) (Debug = True)
  - [log](log/colab.log.v8.log)
  - [feature_importance](data/feature_importance_df.v8.colab.csv)
  
  ![colab feature_importance_v8](https://user-images.githubusercontent.com/50528980/96940780-3ca49100-1496-11eb-917e-4d03a58c6dda.png)

- [20201021-moa-lgbm-benchmark-v12.ipynb](notebooks/20201021-moa-lgbm-benchmark-v12.ipynb)
  - Feature engineering based on feature importance
  - CV:0.05378985351, LB:0.08729  (Debug = True)
  - [log](log/log.v12.log)
  - [feature_importance](data/feature_importance_df.v12.csv)
  
  ![feature_importance_v12](https://user-images.githubusercontent.com/50528980/96957293-e0eefd80-14bf-11eb-9fd5-26b2f8fb7713.png)
  

### [Unfortunately, since my score doesn't reach, and go beyond the public notebook at all, I'll go back to the beginning and reconsider the starter code of U++ san.](kaggle_notebooks/moa-lgbm-benchmark.ipynb)

## 20201023
- [20201023-moa-lgbm-v2.ipynb](notebooks/20201023-moa-lgbm-v2.ipynb)
  - Add debug mode and minor modifications
  - CV:0.01672218, LB:0.02059 (Debug = True)
  - [log](log/log.v2.moa.lgbm.log)

  ![feature_importance_v2](https://user-images.githubusercontent.com/50528980/97024246-335e0780-151c-11eb-91b3-a26fa3ff3049.png)

- [20201023-moa-lgbm-v3.ipynb](notebooks/20201023-moa-lgbm-v3.ipynb)
  - minor modifications, DEBUG=True:CV:0.01672, False:CV:0.01636
  - CV:0.0163661, LB:0.02046 (Debug = False)
  - [log](log/log.v3.moa.lgbm.log)

  ![feature_importance_v3](https://user-images.githubusercontent.com/50528980/97064265-4bf50e80-156a-11eb-8b13-31b8cfc18b63.png)
  
- [20201023-moa-lgbm-v4.ipynb](notebooks/20201023-moa-lgbm-v4.ipynb)
  - minor modifications, DEBUG=True:CV:0.01672, False:CV:0.01636
  - CV:0.01864309, LB:0.02033 (Debug = True)
  - [log](log/log.v4.moa.lgbm.log)

  ![feature_importance_v4](https://user-images.githubusercontent.com/50528980/97064725-b5c2e780-156d-11eb-8519-d289bf6869a9.png)

### I found that the oversampling of the positive cases (minority) and the downsampling of the negative cases (majority) resulted in sample selection bias.

## 20201024
- [20201024-moa-lgbm-benchmark-v13.ipynb](notebooks/20201024-moa-lgbm-benchmark-v13.ipynb)
  - Calibration, SMOTE(k_neighbors=5→1)
  - CV:0.04900, LB:0.05145 (Debug = True)
  - [log](log/log.v13.log)
  - [feature_importance](data/feature_importance_df.v13.csv)
  
  ![feature_importance_v13](https://user-images.githubusercontent.com/50528980/97093231-ca57bc00-160f-11eb-96ca-2cf5e41b3fc3.png)

- [20201024-moa-lgbm-benchmark-v14.ipynb](notebooks/20201024-moa-lgbm-benchmark-v14.ipynb)
  - Removed the Calibration, SMOTE(k_neighbors=1), pasude labeling (thresholds:0.7)
  - CV:0.048851, LB:0.03773 (Debug = True)
  - [log](log/log.v14.log)
  - [feature_importance](data/feature_importance_df.v14.csv)
  
  ![feature_importance_v14](https://user-images.githubusercontent.com/50528980/97096293-b6bc4d80-162f-11eb-85cd-21b5d066e75d.png)

## 20201025
- [20201024-moa-lgbm-benchmark-v15.ipynb](notebooks/20201024-moa-lgbm-benchmark-v15.ipynb)
  -  [Updata anotated data](data/20201024_moa_sig_list.v2.csv)
  - CV:0.044141, LB:0.0391 (Debug = True)
  - [log](log/log.v15.log)
  - [feature_importance](data/feature_importance_df.v15.csv)
  
  ![feature_importance_v15](https://user-images.githubusercontent.com/50528980/97120090-bd57cd00-16e2-11eb-8f6c-0e4cc34658af.png)


