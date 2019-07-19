# SCEBE
# User Manual of 'SCEBE' package 
*Min Yuan*
*2019/07/17*

## Instruction
SCEBE is a R package that conducts high-dimension Genome-Wide Association Study (GWAS) for dynamic traits. The main function in SCEBE package is scebe_sim. Four approaches are included in SCEBE: (1) lme in R package 'lme4' (standard approach), (2) nebe, representing naive empirical Bayes estimation (Londono et al. 2013 and Meirelles et al. 2013), (3) gallop, representing Genome-wide Analysis of Largescale Longitudinal Outcomes using Penalization (GALLOP) (Sikorska et al. 2015), and scebe, representing the proposed two-step simultaneous correction method.

 The source codes and sample data are available at: http://github.com/Myuan2019/SCEBE

## Requirements

SCEBE requires the following R packages:

- MASS, nlme, Matrix, lme4, memoise, dplyr, tidyr, stringr

## Usage

- scebe_sim is the main function
- [phenoData.RData](https://github.com/Myuan2019/SCEBE/blob/master/phenoData.RData) and [genoData. RData](https://github.com/Myuan2019/SCEBE/blob/master/genoData.RData) are the sample data

## Input

- phenoData: a N by 3 dataframe with each row representing a sample; the four columns representing the patient ID, the response variable and the measuring time points, respectively.
> For example:

| ID      |     RAVLT_forgetting| day|
| :-------- |  :-------------: |----:|
| 002_S_0413|                5|    0|
 |002_S_0413|                4 | 190|
 |002_S_0413|                 6 | 389|
 |002_S_0413|                10 | 815|
 |002_S_0413|                4 |1088|
 |...       |...               |... |


- genoData: a large N by q matrix with each row representing  a patient; each column representing an SNP.
  
 - fit0: a lmer object by fitting a base model with phenoData  without any SNPs.
 
 - Time: a character representing the column name of the measuring time points.
 
 - pheno: a character representing the column name of the response variable in the dataset.
 
 - method: a character representing the dynamic GWAS method; can be one of the four: "lme","nebe","gallop" or  "scebe". 
 > (1) “lme”: data are analyzed by lmer in R/lme4; 
 
 > (2) “nebe”: naive empirical Bayes estimation from the base model without covariates;
 
 > (3) “gallop”: Genome-wide Analysis of Largescale Longitudinal Outcomes using Penalization (GALLOP);
 
 > (4) “scebe”: the proposed two stage simultaneous correction method based on EBEs.
 
 ## Output

- parameter estimate, standard error, p-value, running time,  and data preparation time for each SNP effect on intercept (b1) and slope (b2).

- An example for fit0: 
```
fit0 <-lmer(RAVLT_forgetting ~ day + (day | ID), data = Data)

fit0

Linear mixed model fit by REML ['lmerMod']
Formula: RAVLT_forgetting ~ day + (day | ID)
   Data: Data
REML criterion at convergence: 24657.27
Random effects:
 Groups   Name        Std.Dev.  Corr 
 ID       (Intercept) 2.4096533      
          day         0.0008458 -0.73
 Residual             2.0163872      
Number of obs: 5366, groups:  ID, 784
Fixed Effects:
(Intercept)          day  
  4.267e+00   -8.674e-05  
convergence code 0; 3 optimizer warnings; 0 lme4 warnings 

```

- An example of output:
```
s = scebe_sim(phenoData=Data,genoData=geno_mat1,fit0,Time="day, pheno="RAVLT_forgetting",
method = "scebe")

head(s)
               est.sc.b1  se.sc.b1   p.sc.b1     est.sc.b2     se.sc.b2    p.sc.b2
rs137955415  -0.10700076 0.2389157 0.6542544  7.943792e-05 0.0001196067 0.50658835
chr19_94807  -0.06495527 0.2234923 0.7713285  1.909133e-04 0.0001115719 0.08705841
rs56182540    0.13967226 0.2156239 0.5171410 -2.588992e-04 0.0001072818 0.01581034
chr19_96320  -0.10544346 0.2450525 0.6669852  8.807881e-05 0.0001218980 0.46994938
chr19_96365  -0.15106279 0.2577247 0.5577814  1.742659e-04 0.0001252493 0.16411838
chr19_101141  0.01495314 0.3106206 0.9616050 -5.958056e-05 0.0001556056 0.70179780
                    stime       pstime
rs137955415  0.0008090472 0.0004210238
chr19_94807  0.0008090472 0.0004210238
rs56182540   0.0008090472 0.0004210238
chr19_96320  0.0008090472 0.0004210238
chr19_96365  0.0008090472 0.0004210238
chr19_101141 0.0008090472 0.0004210238

```
## References
1.	Londono, D., Chen, K. M., Musolf, A., Wang, R., Shen, T., Brandon, J., ... & Yu, L. A novel method for analyzing genetic association with longitudinal phenotypes. Statistical applications in genetics and molecular biology 12(2), 241-261 (2013). 

2.	Meirelles, O. D., Ding, J., Tanaka, T., Sanna, S., Yang, H. T., Dudekula, D. B., ... & Schlessinger, D. SHAVE: shrinkage estimator measured for multiple visits increases power in GWAS of quantitative traits. European Journal of Human Genetics 21(6), 673 (2013).

3.	Sikorska, K., Lesaffre, E., Groenen, P.J., Rivadeneira, F. and Eilers, P.H., 2018. Genome-wide Analysis of Large-scale Longitudinal Outcomes using Penalization—GALLOP algorithm. Scientific reports, 8(1), 6815.
