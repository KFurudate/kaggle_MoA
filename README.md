# kaggle_MoA

<img width="997" alt="Screen Shot 2020-10-08 at 14 44 57" src="https://user-images.githubusercontent.com/50528980/95506038-eeab6b80-0974-11eb-9a91-29613411768e.png">

https://www.kaggle.com/c/lish-moa

Evaluation: log loss

In this competition, we have a dataset that combines gene expression and cell viability data. 
The data were measured simultaneously human cells’ responses to drugs in a pool of 100 different cell types.
Our aim is to solve the issue of identifying ex-ante, which cell types are better suited for a given drug.in this dataset.

cell viability means that cell lines that have each been labelled with a unique barcode are pooled and treated with the experimental condition, and surviving cells are “counted” through identification of the barcode.

Both genes and cell viability measures are based on the same cell lines.


### [The main idea of my solution](notebooks/20201129-moa-lgbm-v95-inf.ipynb)
  - Single target with gradient boosting decision tree model to train and inference.
  - No GPU because of runtime limit within only 2 hours. → lightgbm
  - Divided into two stages of notebooks. first stage is the feature selection. second stage is train & inference with pseudo label.
  - Selected 300-500 important features for each target variable (first stage).
  - Corrected unbalanced data with oversampling (SMOTE) and undersampling with train_drug (newly added data) or mutually exclusive targets variables.
  - Picked up unpredictable target variables to avoid overfit.
  - Performed pseudo labeling(semi-supervised learning) with reference to the target variable distribution in train data (second stage, some target variable may not be randomly divided, so I hope to predict this well and rank up!).
  
  <img width="1426" alt="Screen Shot 2020-11-28 at 11 53 03" src="https://user-images.githubusercontent.com/50528980/100522679-5eb5c100-3170-11eb-9ac8-70d470886044.png">
  
  <img width="756" alt="Screen Shot 2020-11-30 at 20 22 30" src="https://user-images.githubusercontent.com/50528980/100689386-fc181d00-3349-11eb-8b5c-73b60e5d7c8e.png">

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
### It is not clear whether this makes sense or not. [I decided to find drugs in non scored data that belong to the same category as the data with a low number of targets in the scored data.](data/20201016_moa_sig_list.csv)
  - reference: [薬理学電子教科書](https://drugacademy.atlassian.net/wiki/spaces/PHARMACOLO/overview?mode=global)

  <img width="935" alt="Screen Shot 2020-10-16 at 21 01 12" src="https://user-images.githubusercontent.com/50528980/96326019-ccf45900-0ff2-11eb-8f2f-a511cb0c6598.png">
  
### [I want to use Pseudo Labeling](kaggle_notebooks/pseudo-labeling-qda-0-969.ipynb)
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
### What is the cell viability?
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
  - [Updata anotated data](data/20201024_moa_sig_list.v2.csv)
  - CV:0.044141, LB:0.0391 (Debug = True)
  - [log](log/log.v15.log)
  - [feature_importance](data/feature_importance_df.v15.csv)
  
  ![feature_importance_v15](https://user-images.githubusercontent.com/50528980/97120090-bd57cd00-16e2-11eb-8f6c-0e4cc34658af.png)

## 20201026
- [20201025-moa-lgbm-benchmark-v16.ipynb](notebooks/20201025-moa-lgbm-benchmark-v16.ipynb)
  - Remove noisy label(confidence: 0.9)
  - CV: failure, LB: failure (Debug = True)

## 20201027
- [20201026-moa-lgbm-benchmark-v17.ipynb](notebooks/20201026-moa-lgbm-benchmark-v17.ipynb)
  - Modifications with remove noisy label func, Add Calibration, , confidence = y_prob.probability.max()*0.3
  - CV:0.0452748, LB:0.05224 (Debug = True)
  - [log](log/log.v17.log)
  
  ![feature_importance_v17](https://user-images.githubusercontent.com/50528980/97324917-66680a00-1840-11eb-9213-7a0748f2b648.png)

- [20201027-moa-lgbm-benchmark-v18.ipynb](notebooks/20201027-moa-lgbm-benchmark-v18.ipynb)
  - SMOTE(k_neighbors=1→2), confidence = y_prob.probability.max()*0.2, Calibration
  - CV:0.0386774, LB:0.05382(Debug = True)
  - [log](log/log.v18.log)
  
  ![feature_importance_v18](https://user-images.githubusercontent.com/50528980/97387909-f8a1f980-18a4-11eb-9fc2-6075b94ab517.png)

- [High-throughput identification of genotype-specific cancer vulnerabilities in mixtures of barcoded tumor cell lines](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5508574/)
  - I'll read it later.
  - reference: https://www.kaggle.com/c/lish-moa/discussion/190693
  
## 20201028
- [20201027-moa-lgbm-benchmark-v19.ipynb](notebooks/20201027-moa-lgbm-benchmark-v19.ipynb)
  - SMOTE(k_neighbors=2→3) , confidence = y_prob.probability.max()*0.2, Calibration
  - CV:0.0389749, LB:0.05388 (Debug = True)
  - [log](log/log.v19.log)
  
  ![feature_importance_v19](https://user-images.githubusercontent.com/50528980/97471386-a8fc1600-1916-11eb-8f03-80221a06b311.png)
  
- [20201027-moa-lgbm-benchmark-v20.ipynb](notebooks/20201027-moa-lgbm-benchmark-v20.ipynb)
  - Modifications with pseudo_labeling func, Removed the Calibration, SMOTE(k_neighbors=2), 
  - CV:0.0281714, LB:0.10660 (Debug = True)
  - [log](log/log.v20.log)
  
  ![feature_importance_v20](https://user-images.githubusercontent.com/50528980/97493438-f2f2f500-1932-11eb-82ca-8a42f4305395.png)
  
- CV improves, but LBs are bad. I think this is due to the inclusion of low confidence pseudo labels in the TEST data.
  - reference: https://www.kaggle.com/c/lish-moa/discussion/191135
  <img width="652" alt="Screen Shot 2020-10-29 at 13 57 25" src="https://user-images.githubusercontent.com/50528980/97619961-e2a05000-19ee-11eb-94c3-92c0ce5b9e84.png">

## 20201029
- [20201028-moa-lgbm-benchmark-v21.ipynb](notebooks/20201028-moa-lgbm-benchmark-v21.ipynbb)
  - minor modifications
  - CV:0.02459585, LB:0.0760 (Debug = False)
  - [log](log/log.v21.log)
  
  ![feature_importance_v21](https://user-images.githubusercontent.com/50528980/97635109-d1fad480-1a04-11eb-880d-59b9e87e461d.png)

- [20201029-moa-lgbm-benchmark-v22.ipynb](notebooks/20201029-moa-lgbm-benchmark-v22.ipynb)
  - minor modifications
  - CV:0.028116, LB:0.12838 (Debug = True)
  - [log](log/log.v22.log)

- [20201029-moa-lgbm-benchmark-v23.ipynb](notebooks/20201029-moa-lgbm-benchmark-v23.ipynb)
  - TOP100→PCA→UMAP(n_components=3)
  - CV:0.03065106, LB:0.46979 (Debug = True)
  - [log](log/log.v23.log)
  
  ![feature_importance_v23](https://user-images.githubusercontent.com/50528980/97615379-39a32680-19e9-11eb-8571-84caa7342fec.png)

- [20201029-moa-lgbm-benchmark-v24.ipynb](notebooks/20201029-moa-lgbm-benchmark-v24.ipynb)
  - TOP100→PCA→UMAP(n_components=10), UMAP(n_components=2→3)
  - CV:0.02892854, LB:0.46979 (Debug = True)
  - [log](log/log.v24.log)
  
  ![feature_importance_v24](https://user-images.githubusercontent.com/50528980/97626013-4a5a9900-19f7-11eb-8b0a-aefad298d2da.png)

- [20201029-moa-lgbm-benchmark-v25.ipynb](notebooks/20201029-moa-lgbm-benchmark-v25.ipynb)
  - Feature engineering based on Feature importance
  - CV:0.02595783, LB: (Debug = True)
  - [log](log/log.v25.log)
  
  ![feature_importance_v25](https://user-images.githubusercontent.com/50528980/97635156-e5a63b00-1a04-11eb-97eb-2249a43b9613.png)

- [20201029-moa-lgbm-benchmark-v26.ipynb](notebooks/20201029-moa-lgbm-benchmark-v26.ipynb)
  - Feature engineering based on Feature importance
  - CV: 0.061961, LB: 0.48935 (Debug = True)
  - [log](log/log.v26.log)

  ![feature_importance_v26](https://user-images.githubusercontent.com/50528980/97651460-1e0c4000-1a2a-11eb-878e-bfcbb00c6a7f.png)

- [20201029-moa-lgbm-benchmark-v28.ipynb](notebooks/20201029-moa-lgbm-benchmark-v28.ipynb)
  - LGBMClassifie:clf.predict→clf.predict_proba, with Calibration 
  - CV: 0.066562, LB: 0.66135 (Debug = True)
  - [log](log/log.v28.log)
  
  ![feature_importance_v28](https://user-images.githubusercontent.com/50528980/97657614-531f8f00-1a38-11eb-9286-f4f3ae6d0e80.png)

## 20201030
- [20201029-moa-lgbm-benchmark-v29.ipynb](notebooks/20201029-moa-lgbm-benchmark-v29.ipynb)
  - Remove Calibration, is_unbalance': True, SMOTE(k_neighbors=2→3), Modify pseudo labeling func to include low confidence pseudo labels in the TEST data, target_rate *= 1.2 
  - CV: 0.030911, LB: 0.68009(Debug = True)
  - [log](log/log.v29.log)
  
  ![feature_importance_v29](https://user-images.githubusercontent.com/50528980/97722407-9c9fc680-1a98-11eb-8c81-d2f267813ef6.png)

- [20201030-moa-lgbm-benchmark-v30.ipynb](notebooks/20201030-moa-lgbm-benchmark-v30.ipynb)
  - drop_duplicates(keep="last"), target_rate *= 1.2 
  - CV: 0.03085, LB: 0.68007 (Debug = True)
  - [log](log/log.v30.log)
  
  ![feature_importance_v30](https://user-images.githubusercontent.com/50528980/97768806-b4586880-1af3-11eb-9110-f8f3b79c8d18.png)

## 20201031
- [20201030-moa-lgbm-benchmark-v31.ipynb](notebooks/20201030-moa-lgbm-benchmark-v31.ipynb)
  - target_rate *= 1.1, if Threshold <= 0.2: break, if sum(p_label)*1.5 >= check: break, if sum(p_label) <= check*1.5: break
  - CV:0.0282074(0.029355), LB: 0.68001 (Debug = True)
  - [log](log/log.v31.log)
  
  ![feature_importance_v31](https://user-images.githubusercontent.com/50528980/97779960-96205600-1b4f-11eb-822c-61c3ed1af0e4.png)
  
### Thanks for this discussion, assuming there are at least six groups of drugs, we can know what some of the drugs. But, how can we apply this to our learning?

  - reference: https://www.kaggle.com/c/lish-moa/discussion/194190 

  <img width="789" alt="Screen Shot 2020-10-31 at 7 33 16" src="https://user-images.githubusercontent.com/50528980/97779429-ff05cf00-1b4b-11eb-98f9-189574465af2.png">
  
  <img width="657" alt="Screen Shot 2020-10-31 at 7 34 07" src="https://user-images.githubusercontent.com/50528980/97779456-31173100-1b4c-11eb-8ced-46e962aff0b6.png">
  
### I want to know what is the distribution of targets in the test data. I hope this is the same as the train. But if it's really randomly split, I believe that the target distribution will be about the same for train and test.

  - reference: https://www.kaggle.com/c/lish-moa/discussion/193907
  
  <img width="618" alt="Screen Shot 2020-10-31 at 13 04 18" src="https://user-images.githubusercontent.com/50528980/97786581-82d6b000-1b7a-11eb-864d-092a0bdbd4ea.png">

### I'll try Runk Gauss scaling
  - reference: https://www.kaggle.com/c/lish-moa/discussion/193878

  <img width="646" alt="Screen Shot 2020-10-31 at 17 51 25" src="https://user-images.githubusercontent.com/50528980/97791572-c85aa380-1ba1-11eb-96f6-ba8fd1c613bf.png">
  
  <img width="577" alt="Screen Shot 2020-11-01 at 8 39 14" src="https://user-images.githubusercontent.com/50528980/97805790-d5b57380-1c1d-11eb-8f3e-cbe6e078ed0e.png">

## 20201101
- [20201101-moa-lgbm-benchmark-v32.ipynb](notebooks/20201101-moa-lgbm-benchmark-v32.ipynb)
  - y_prob.probability.quantile(0.3), if Threshold >= 0.95: break
  - CV:0.029914(0.029355), LB:no sub (Debug = True)
  - [log](log/log.v32.log)

  ![feature_importance_v32](https://user-images.githubusercontent.com/50528980/97793537-5727ea00-1bbb-11eb-9e15-ada90797032e.png)


- [20201031-moa-lgbm-benchmark-v33.ipynb](notebooks/20201031-moa-lgbm-benchmark-v33.ipynb)
  - RankGauss, Scaled by category, SMOTE(k_neighbors=2,
  - CV:0.027925299, LB:0.03448 (Debug = True)
  - [log](log/log.v33.log)

  ![feature_importance_v33](https://user-images.githubusercontent.com/50528980/97913131-83567e80-1d13-11eb-81ad-c6187d77baa9.png)
  
## 20201102
- [20201102-moa-lgbm-benchmark-v35.ipynb](notebooks/20201102-moa-lgbm-benchmark-v35.ipynb)
  - f_div = lambda x: ((x+d)*10 / (abs(med)+d))**2, f_diff = lambda x: ((x-med)*10)**2, select features
  - CV:0.03244971, LB: Notebook Timeout(Debug = True)
  - [log](log/log.v35.log)
  
  ![all_feature_importance_v35](https://user-images.githubusercontent.com/50528980/97951012-4e234e00-1d5e-11eb-86b1-158935ee8bcb.png)
    
## 20201103
- [20201102-moa-lgbm-v36.ipynb](notebooks/20201102-moa-lgbm-v36.ipynb)
  - Add feature importance
  - CV:0.034882, LB: Cannot submit with internet access  (Debug = True)
  - [log](log/log.v36.log)
  
  ![all_feature_importance_v36](https://user-images.githubusercontent.com/50528980/98018360-0e904c80-1dc6-11eb-8b53-ba9c531bf33a.png)
  
## 20201104
- [20201103-moa-lgbm-v37.ipynb](notebooks/20201103-moa-lgbm-v37.ipynb)
  - Remove RankGauss for gene expression, fix feature importance func
  - CV:0.0327302, LB: 0.03264 (Debug = True)
  - [log](log/log.v37.log)
  - [importance_cols_df.v37.csv](/data/)
  - [feature_importance_df.v37.csv](/data/)
  
  ![all_feature_importance_v37](https://user-images.githubusercontent.com/50528980/98200142-91162a80-1ef2-11eb-8714-7b34f413c3cd.png)

### New data file available: train_drug.csv
  - Drug and MultiLabel Stratification Code
  - reference: https://www.kaggle.com/c/lish-moa/discussion/195195
  - reference: https://www.kaggle.com/c/lish-moa/discussion/195170
  
  <img width="324" alt="Screen Shot 2020-11-03 at 20 30 42" src="https://user-images.githubusercontent.com/50528980/98200300-f0743a80-1ef2-11eb-885e-5ef2357c034c.png">
  
### Interesting comment by host
  - reference: https://www.kaggle.com/c/lish-moa/discussion/195170
  
  <img width="649" alt="Screen Shot 2020-11-03 at 22 42 22" src="https://user-images.githubusercontent.com/50528980/98200661-b0fa1e00-1ef3-11eb-8504-e033ca32f3dc.png">

- [20201103-moa-lgbm-v38.ipynb](notebooks/20201103-moa-lgbm-v38.ipynb)
  - Add MultiLabel Stratification func, fix index of data before split with "data = data.sort_index(axis='index')"

- [20201103-moa-lgbm-v39-2.ipynb](notebooks/20201103-moa-lgbm-v39-2.ipynb)
  - fix pseudo labeling func
  - CV:0.0193437, LB:0.04470 (Debug = True)
  - [log](log/log.v39.log)
  - [importance_cols_df.v39.csv](/data/)
  - [feature_importance_df.v39.csv](/data/)
  
  ![all_feature_importance_v39](https://user-images.githubusercontent.com/50528980/98295089-9cab3500-1f76-11eb-8716-b8632cd81369.png)

## 20201105
- [20201103-moa-lgbm-v40.ipynb](notebooks/20201103-moa-lgbm-v40.ipynb)
  - fix pseudo labeling func, select_importance_cols(feature_importance_df, num=30)
  - CV:0.0201289, LB: (Debug = True)
  - [log](log/log.v40.log)
  - [importance_cols_df.v40.csv](/data/)
  - [feature_importance_df.v40.csv](/data/)
  
  ![all_feature_importance_v40](https://user-images.githubusercontent.com/50528980/98457315-82768000-214b-11eb-9881-44ab5ee46100.png)
  
- [20201105-moa-lgbm-v41.ipynb](notebooks/20201105-moa-lgbm-v41.ipynb)
  - Feature engineering based on Feature importance with v39 notebook
  - CV:0.0150337, LB:0.68323 (Debug = True)
  - [log](log/log.v40.log)

## 20201106
- [20201106-moa-lgbm-inference-v42-1.ipynb](notebooks/20201106-moa-lgbm-inference-v42-1.ipynb)
  - Feature engineering based on Feature importance with v40 notebook
  - Remove Kolmogorov-Smirnov test and VarianceThreshold for inference, Threshold = np.quantile(y_prob, q=0.99)
  - CV:0.02704226, LB: (Debug = True)
  - [log](log/log.v41.log)
  
- [20201103-moa-lgbm-v40.2.ipynb](notebooks/20201103-moa-lgbm-v40.ipynb)
  - fix pseudo labeling func, select_importance_cols(feature_importance_df, num=30)
  - CV:0.020234, LB: (Debug = True)
  - [log](log/log.v40.2.log)
  - [importance_cols_df.v40.2.csv](/data/)
  - [feature_importance_df.v40.2.csv](/data/)
  
  ![all_feature_importance_v40 2](https://user-images.githubusercontent.com/50528980/98457504-9d49f400-214d-11eb-9c2b-c8fb26194f30.png)
  
### I decided to use two notebooks. First, a select notebook that picks up important features for each target. Second, the inference notebook does train and inference with selected important features.

- [20201106-moa-lgbm-inference-v43.ipynb](notebooks/20201106-moa-lgbm-inference-v43.ipynb)
  - Inference, modify pseudo labeling func: lower, upper, change param 3→2.1
  - CV:0.032565, LB: 0.05682 (Debug = True)
  - [log](log/log.v43.log)
  
- [20201106-moa-lgbm-inference-v44-2.ipynb](notebooks/20201106-moa-lgbm-inference-v44-2.ipynb)
  - Inference based on v40.2 note book(updated fe_stats func), Debug=False, modify  eature_Engineering func, check*(3.5-len(str(int(check))))
  - CV:0.024157, LB: 0.05774 (Debug=False)
  - [log](log/log.v44.2.log)

## 20201107
- [20201107-moa-lgbm-v45-inf.ipynb](notebooks/20201107-moa-lgbm-v45-inf.ipynb)
  - inference used v40.2, DEBUG=False, Add create_features func,  learning_rate(pseudo labeling): 0.01→0.02
  - CV:0.0248876, LB: 0.05574 (Debug = True)
  - [log](log/log.v45.log)

## 20201108
- [20201107-moa-lgbm-v46-sel.ipynb](notebooks/20201107-moa-lgbm-v46-sel.ipynb)
  - Select, Add 'g_d_g', 'c_d_c', 'g_df_g', 'c_df_c', 'd_g_df_g', 'd_c_df_c' to the kind_list, VarianceThreshold(0.91) 
  - CV:0.020792, LB: (Debug = True)
  - [log](log/log.v46.log)
  - [importance_cols_df.v46.csv](/data/)
  - [feature_importance_df.v46.csv](/data/)
  
  ![all_feature_importance_v46](https://user-images.githubusercontent.com/50528980/98615621-b45d2300-22c0-11eb-9f23-9a59b410dab0.png)

## 20201109
- [20201109-moa-lgbm-v48-inf.ipynb](notebooks/20201109-moa-lgbm-v48-inf.ipynb)
  - inference used v46, DEBUG=False, modify Feature_Engineering func
  - CV:0.02425838, LB:Notebook Timeout  (DEBUG=False)
  - [log](log/log.v48.log)
  
# 20201110
- [20201109-moa-lgbm-v49-inf.ipynb](notebooks/20201109-moa-lgbm-v49-inf.ipynb)
  - inference used v46, DEBUG=False, create_features(_X_data, selected_cols, func="mean")
  - CV:0.02425, LB:Notebook Timeout (DEBUG=False)
  - [log](log/log.v49.log)
  
## 20201111
- [20201109-moa-lgbm-v50-sel.ipynb](notebooks/20201109-moa-lgbm-v50-sel.ipynb)
  - Select, QuantileTransformer(n_quantiles=100), VarianceThreshold(0.9)  
  - CV:0.021026, LB: (Debug = True)
  - [log](log/log.v50.log)
  - [importance_cols_df.v50.csv](/data/)
  - [feature_importance_df.v50.csv](/data/)
  
  ![all_feature_importance_v50](https://user-images.githubusercontent.com/50528980/98832760-d19a0a80-2402-11eb-86fc-3aa7bb1663bc.png)
  
- [20201110-moa-lgbm-v51-sel.ipynb](notebooks/20201110-moa-lgbm-v51-sel.ipynb)
  - Select, QuantileTransformer(n_quantiles=defalt), VarianceThreshold(0.9)  
  - CV:0.020668, LB: (Debug = True)
  - [log](log/log.v51.log)
  - [importance_cols_df.v51.csv](/data/)
  - [feature_importance_df.v51.csv](/data/)
  
  ![all_feature_importance_v51](https://user-images.githubusercontent.com/50528980/98835200-bda3d800-2405-11eb-9056-1fb1517485fe.png)

- [20201110-moa-lgbm-v52-inf.ipynb](notebooks/20201110-moa-lgbm-v52-inf.ipynb)
  - inference used v46, adjust params in pseudo_labeling func
  - CV:0.024201, LB:0.06431 (DEBUG=False)
  - [log](log/log.v52.log)

- [20201111-moa-lgbm-v53-sel.ipynb](notebooks/20201111-moa-lgbm-v53-sel.ipynb) 
  - Select, feature engining→VarianceThreshold(0.98), select_importance_cols(num=200)
  - CV:0.01995, LB: (Debug = True)
  - [log](log/log.v53.log)
  - [importance_cols_df.v53.csv](/data/)
  - [feature_importance_df.v53.csv](/data/)
  
  ![all_feature_importance_v53](https://user-images.githubusercontent.com/50528980/98951768-45e6b380-24c0-11eb-905c-0b6511141bb8.png)
 
## 20201112
- [20201111-moa-lgbm-v54-inf.ipynb](notebooks/20201111-moa-lgbm-v54-inf.ipynb)
  - inference used v46, adjust params in pseudo_labeling func
  - CV:0.0232771, LB:0.06414 (DEBUG=False)
  - [log](log/log.v54.log)
  
- [20201111-moa-lgbm-v55-inf.ipynb](notebooks/20201111-moa-lgbm-v55-inf.ipynb)
  - inference used v51, Not using create_cluster func
  - CV:0.02126, LB:Notebook Timeout (DEBUG=False)
  - [log](log/log.v55.log)

- [20201112-moa-lgbm-v58-inf.ipynb](notebooks/20201112-moa-lgbm-v58-inf.ipynb)
  - inference used v53, 'lambda_l2':3
  - CV:0.02191, LB:0.03982 (DEBUG=False)
  - [log](log/log.v58.log)

## 20201113
- [20201113-moa-lgbm-v59-inf.ipynb](notebooks/20201113-moa-lgbm-v59-inf.ipynbb)
  - inference used v53, 'lambda_l2':15
  - CV:0.02301, LB: (DEBUG=False)
  - [log](log/log.v59.log)
  
- [20201113-moa-lgbm-v60-inf.ipynb](notebooks/20201113-moa-lgbm-v60-inf.ipynb)
  - inference used v53, 'lambda_l2':0.1,'bagging_fraction': 0.7, 'bagging_freq': 1, 'learning_rate': 0.03,
  - CV:0.020481, LB:0.04024 (DEBUG=False)
  - [log](log/log.v60.log)
  
- [20201113-moa-lgbm-v61-inf.ipynb](notebooks/20201113-moa-lgbm-v61-inf.ipynb)
  - inference used v53, 'lambda_l2':3,'bagging_fraction': 0.7, 'bagging_freq': 1, 'learning_rate': 0.03,
  - CV:0.02148, LB: (DEBUG=False)
  - [log](log/log.v61.log)
  
### very usefull.
<img width="732" alt="Screen Shot 2020-11-14 at 18 57 32" src="https://user-images.githubusercontent.com/50528980/99160294-6161e200-26ab-11eb-8107-b66c6d95ec52.png">

[Model Evaluation, Model Selection, and Algorithm Selection in Machine Learning](https://github.com/KFurudate/kaggle_MoA/blob/main/Related%20papers/Model%20Evaluation%2C%20Model%20Selection%2C%20and%20Algorithm%20Selection%20in%20Machine%20Learning.pdf)

## 20201114
- [20201114-moa-lgbm-v62-inf.ipynb](notebooks/20201114-moa-lgbm-v62-inf.ipynb)
  - inference used v53, self-Stacking prediction values, Threshold += 0.05, 'learning_rate': 0.05, 'lambda_l2':3
  - CV:0.0023, LB: 0.14471 (DEBUG=False)
  - [log](log/log.v62.log)

- [20201114-moa-lgbm-v63-inf.ipynb](notebooks/20201114-moa-lgbm-v63-inf.ipynb)
  - inference used v53, 'lambda_l2':15,
  - CV:0.00257, LB: 0.1458) (DEBUG=False)
  - [log](log/log.v63.log)

## 20201115
- [20201115_MoA_lgbm.v64.inf.ipynb](notebooks/20201115_MoA_lgbm.v64.inf.ipynb)
  - inference used v53, if iter_ == max_iter: X_test["pred_feat"] = y_prob.copy()
  - CV:0.00227, LB: 0.1050 (DEBUG=False)
  - [log](log/log.v64.log)
  
- [20201115-moa-lgbm-v65-inf.ipynb](notebooks/20201115-moa-lgbm-v65-inf.ipynb)
  - inference used v53, Apply RankGauss(df) to "pred_feat"
  - CV:0.00219, LB: 0.50053 (DEBUG=False)
  - [log](log/log.v65.log)

## 20201116
- [20201116-moa-lgbm-v66-inf.ipynb](notebooks/20201116-moa-lgbm-v66-inf.ipynb)
  - inference used v53, Apply clipping & RankGauss(df) to "pred_feat"
  - CV:0.00219, LB: 0.51163 (DEBUG=False)
  - [log](log/log.v66.log)
  
- [20201116-moa-lgbm-v67-sel.ipynb](notebooks/20201116-moa-lgbm-v67-sel.ipynb) 
  - Select, VarianceThreshold(0.98), select_importance_cols(num=300)
  - CV:0.0197582, LB: (Debug = True)
  - [log](log/log.v67.log)
  - [importance_cols_df.v67.csv](/data/)
  - [feature_importance_df.v67.csv](/data/)
  
  ![all_feature_importance_v67](https://user-images.githubusercontent.com/50528980/99589558-c40bf400-29b1-11eb-8886-e327749737bf.png)
  
- [20201116-moa-lgbm-v68-inf.ipynb](notebooks/20201116-moa-lgbm-v68-inf.ipynb)
  - inference used v67, np.clip
  - CV:0.021389, LB: 0.04027 (DEBUG=False)
  - [log](log/log.v67.log)
  
## 20201117
- [20201117-moa-lgbm-v69-inf.ipynb](notebooks/20201117-moa-lgbm-v69-inf.ipynb)
  - inference used v67, if _score > 0.02: np.clip
  - CV:0.020943, LB: 0.04068 (DEBUG=False)
  - [log](log/log.v69.log)

- [20201117-moa-lgbm-v70-inf.ipynb](notebooks/20201117-moa-lgbm-v70-inf.ipynb)
  - inference used v67, adjust prediction
  - CV:0.02094, LB: 0.03969 (DEBUG=False)
  - [log](log/log.v70.log)

- [20201117_MoA_lgbm.v71.inf.ipynb](notebooks/20201117-moa-lgbm-v71-inf.ipynb)
  - inference used v67, (Threshold+0.03) >= 0.96
  - CV:0.02094, LB: 0.03969 (DEBUG=False)
  - [log](log/log.v71.log)

## 20201118
- [20201117-moa-lgbm-v72-inf.ipynb](notebooks/20201117-moa-lgbm-v72-inf.ipynb)
  - inference used v67, remove "if (p_label_rate*1.5) < target_rate" from the Adj_threshold func
  - CV:0.020943, LB: 0.04131 (DEBUG=False)
  - [log](log/log.v72.log)

### I strongly agree with him. Multi predicts by capturing the relationship between co-occurrence and exclusion, not the presence or absence of a target. On the other hand, single predicts with or without a target. 

   - reference: https://www.kaggle.com/c/lish-moa/discussion/197855

  <img width="752" alt="Screen Shot 2020-11-18 at 10 57 53" src="https://user-images.githubusercontent.com/50528980/99561832-0242ec80-298d-11eb-8618-5eed0b414516.png">
  
   - reference: https://www.kaggle.com/c/lish-moa/discussion/197491
   
  <img width="773" alt="Screen Shot 2020-11-22 at 22 47 24" src="https://user-images.githubusercontent.com/50528980/99930182-c190fd80-2d15-11eb-8c9f-41cf3a2b78ba.png">
  
## 20201119
- [20201118-moa-lgbm-v73-inf.ipynb](notebooks/20201118-moa-lgbm-v73-inf.ipynb)
  - inference used v67, exclusive_cols→Under_Sampling func
  - CV:0.017074, LB: 0.03214 (DEBUG=False)
  - [log](log/log.v73.log)
  
## 20201120
### [Calculated the relationship between co-occurrence and exclusion with R. I posted it in the Kaggle discussion but deleted it after a day with no response from anyone](https://github.com/KFurudate/kaggle_MoA/blob/main/data/20201118_train_targets_scored.logodds.pdf)
  - [20201118_train_targets_scored.matrix_odds_results.csv](/data/)

  <img width="755" alt="Screen Shot 2020-11-20 at 18 23 36" src="https://user-images.githubusercontent.com/50528980/99930717-af17c380-2d17-11eb-9430-b5225c8161ed.png">
  <img width="1680" alt="Screen Shot 2020-11-22 at 23 12 14" src="https://user-images.githubusercontent.com/50528980/99930858-382efa80-2d18-11eb-9d08-e02e60e30562.png">

## 20201121
- [20201121-moa-lgbm-v74-inf.ipynb](notebooks/20201121-moa-lgbm-v74-inf.ipynb)
  - inference used v67, feature engining for unpredictable_cols(create_features func 30)
  - CV:0.017074, LB: 0.03224 (DEBUG=False)
  - [log](log/log.v74.log)

## 20201122
- [20201122-moa-lgbm-v75-inf.ipynb](notebooks/20201122-moa-lgbm-v75-inf.ipynb)
  - inference used v67, avoid overfit for unpredictable_cols
  - CV:0.016922, LB: 0.03205 (DEBUG=False)
  - [log](log/log.v75.log)
 
- [20201122-moa-lgbm-v76-inf.ipynb](notebooks/20201122-moa-lgbm-v76-inf.ipynb)
  - inference used v67, 'feature_fraction':0.1, 'lambda_l2': 200,
  - CV:0.01718, LB: Notebook Timeout (DEBUG=False)
  - [log](log/log.v76.log)

## 20201123
- [20201123-moa-lgbm-v77-inf.ipynb](notebooks/20201123-moa-lgbm-v77-inf.ipynb)
  - inference used v67, Add pca to create_features func 
  - CV:0.01710, LB: Notebook Timeout (DEBUG=False)
  - [log](log/log.v77.log)

## 20201124
- [20201124-moa-lgbm-v78-inf.ipynb](notebooks/20201124-moa-lgbm-v78-inf.ipynb)
  - inference used v67,adjust lgm params; 'lambda_l2': 300, 'bagging_fraction': 0.7,'colsample_bytree': 0.05,'subsample': 0.7,'reg_lambda': 100,
  - CV:0.01763, LB: 0.03170 Timeout (DEBUG=False)
  - [log](log/log.v78.log)

- [20201124-moa-lgbm-v79-inf.ipynb](notebooks/20201124-moa-lgbm-v79-inf.ipynb)
  - inference used v67, adjust lgbm params; 'lambda_l2': 300, 'bagging_fraction': 0.7,'colsample_bytree': 0.04,　'subsample': 0.7,'reg_lambda': 200,
  - CV:0.01725, LB: 0.03180 Timeout (DEBUG=False)
  - [log](log/log.v79.log)
  
### I hope shake up happens because my notebooks tackle the issue.
  - reference: https://www.kaggle.com/c/lish-moa/discussion/199484
  <img width="735" alt="Screen Shot 2020-11-28 at 13 21 41" src="https://user-images.githubusercontent.com/50528980/100524320-c70a9f80-317c-11eb-8636-a755b1a903d3.png">
 
## 20201125
- [20201125-moa-lgbm-v80-inf.ipynb](notebooks/20201125-moa-lgbm-v80-inf.ipynb)
  - inference used v67, lgm params used v78, N_FOLD = 5
  - CV:0.01741, LB: 0.03150 Timeout (DEBUG=False)
  - [log](log/log.v80.log)
  
- [20201125-moa-lgbm-v81-inf.ipynb](notebooks/20201125-moa-lgbm-v81-inf.ipynb)
  - inference used v67, lgm params used v78, N_FOLD = 5&Early_stopping_rounds=29, unpredictable_cols→ N_FOLD = 3 & Early_stopping_rounds=39
  - CV:0.01747, LB: 0.03161 Timeout (DEBUG=False)
  - [log](log/log.v81.log)

- [20201125-moa-lgbm-v82sel.ipynb](notebooks/20201125-moa-lgbm-v82sel.ipynb) 
  - Select, DEBUG = True, 'feature_fraction':0.007, select_importance_cols(num=500), 
  - CV:0.023377, LB: (Debug = True)
  - [log](log/log.v82.log)
  - [importance_cols_df.v82.csv](/data/)
  - [feature_importance_df.v82.csv](/data/)
  
  ![all_feature_importance_v82](https://user-images.githubusercontent.com/50528980/100404225-813bc300-3026-11eb-971b-8d3cb4163f5f.png)

## 20201126
- [20201125-moa-lgbm-v83-inf.ipynb](notebooks/20201125-moa-lgbm-v83-inf.ipynb)
  - inference used v67, lgm params used v79, odds_gene_df["Log"] < 1.0→0.0
  - CV:0.017458, LB: (DEBUG=False)
  - [log](log/log.v83.log)
  
- [20201125-moa-lgbm-v84-sel.ipynb](notebooks/20201125-moa-lgbm-v84-sel.ipynb)
  - Select, based on v83, DEBUG = False, 'feature_fraction':0.007, select_importance_cols(num=300),
  - It did not end within the time limit. I tried it locally with GPU, but when the learning was 65%,  it ran out of memory and stop.

- [20201125-moa-lgbm-v85-inf.ipynb](notebooks/20201125-moa-lgbm-v85-inf.ipynb)
  - inference used v82, based on v83
  - CV:0.018733, LB:0.03114 (DEBUG=False)
  - [log](log/log.v85.log)

- [20201126-moa-lgbm-v86-inf.ipynb](notebooks/20201126-moa-lgbm-v86-inf.ipynb)
  - inference used v82, based on v83
  - CV:0.01928, LB: Notebook Timeout (DEBUG=False)
  - [log](log/log.v86.log)
  
## 20201127
- [20201125-moa-lgbm-v87-inf.ipynb](notebooks/20201125-moa-lgbm-v87-inf.ipynb)
  - inference used v82, based on v85, unpredictable_cols=()
  - CV:0.0218119, LB: 0.03124(DEBUG=False)
  - [log](log/log.v87.log)

- [20201125-moa-lgbm-v88-inf.ipynb](notebooks/20201125-moa-lgbm-v88-inf.ipynb)
  - inference used v82, based on v85,unpredictable_cols based on v87
  - CV:0.019748, LB: Notebook Timeou (DEBUG=False)
  - [log](log/log.v88.log)
 
### Combined selected top features based on log loss score by version 67 and version 82 notebooks for better prediction.
- [importance_cols_df.v67_v82.csv](/data/)

## 20201128
### From single target model perspective, I think his "Hard Targets" means would be target variables that are likely to over-fitting with created features by each other.
- reference: https://www.kaggle.com/c/lish-moa/discussion/199696 
<img width="735" alt="Screen Shot 2020-11-28 at 13 05 13" src="https://user-images.githubusercontent.com/50528980/100524057-8578f500-317a-11eb-95c4-cd9025c24e6e.png">

- [20201128-moa-lgbm-v89-inf.ipynb](notebooks/20201128-moa-lgbm-v89-inf.ipynb)
  - inference used v67 & v82, based on v85, unpredictable_cols=()
  - CV:0.0155371, LB: 0.03194(DEBUG=False)
  - [log](log/log.v88.log)

- [20201128-moa-lgbm-v90-inf.ipynb](notebooks/20201128-moa-lgbm-v90-inf.ipynb)
  - inference used v67&v82, based on v85, unpredictable_cols=(), if _score > 0.02:unpredictable.append(target_col)
  - CV:0.0155371, LB: 0.03194(DEBUG=False)
  - [log](log/log.v90.log)
  
- [20201128-moa-lgbm-v91-inf.ipynb](notebooks/20201128-moa-lgbm-v91-inf.ipynb)
  - inference used v67&v82, based on v85, unpredictable_cols based on v89, 'colsample_bytree': 0.05,
  - CV:0.01353, LB: 0.03161(DEBUG=False)
  - [log](log/log.v91.log)

- [20201128-moa-lgbm-v92-inf.ipynb](notebooks/20201128-moa-lgbm-v92-inf.ipynb)
  - inference used v67&v82, based on v85, unpredictable_cols based on v90, 'colsample_bytree': 0.04,
  - CV:0.01424, LB: 0.03182(DEBUG=False)
  - [log](log/log.v92.log)
 
- [20201128-moa-lgbm-v93-inf.ipynb](notebooks/20201128-moa-lgbm-v93-inf.ipynb)
  - inference used v67&v82, based on v85, unpredictable_cols based on v89&v90,
  - CV:0.0125883, LB: 0.03156(DEBUG=False)
  - [log](log/log.v93.log)

## 20201129
- [20201128-moa-lgbm-v94-inf.ipynb](notebooks/20201128-moa-lgbm-v94-inf.ipynb)
  - inference used v67&v82, based on v93,
  - CV:0.012560, LB:0.03157 (DEBUG=False)
  - [log](log/log.v94.log)

- [20201129-moa-lgbm-v95-inf.ipynb](notebooks/20201129-moa-lgbm-v95-inf.ipynb)
  - inference used v67&v82, based on v94,
  - CV:0.0125621, LB:0.03146 (DEBUG=False)
  - [log](log/log.v95.log)
  
### In LB could not exceed at all the score of U++'s notebook, which was the baseline.
  <img width="733" alt="Screen Shot 2020-11-30 at 8 06 27" src="https://user-images.githubusercontent.com/50528980/100620426-09022580-32e4-11eb-9114-702fe3974c32.png">
  
  <img width="756" alt="Screen Shot 2020-11-30 at 20 22 30" src="https://user-images.githubusercontent.com/50528980/100689386-fc181d00-3349-11eb-8b5c-73b60e5d7c8e.png">
