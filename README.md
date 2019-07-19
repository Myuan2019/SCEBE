# SCEBE
# User Manual of 'SCEBE' package 
*Min Yuan*
*2019/07/17*

## Instruction
The main function in SCEBE package is scebe_sim which can fit a linear mixed effects model with four commonly used approaches including *lme* in R package 'lme4',*nebe* for naieve empirical Bayes estimation, *gallop* for GALLOP and *scebe* for the proposed two-step simultaneous correction method. 

The source codes and sample data are available at (http://github.com/Myuan2019//SCEBE)

## Requirements

SCEBE requires the following R packages:

- MASS, nlme, Matrix, lme4, memoise, dplyr, tidyr, stringr

## Usage

- scebe_sim is the main function
- example.RData is the sample data

## Input

- phenoData: a N by 4 dataframe with each row representing a sample; the four columns representing the patient ID, measuring time points, the response variable and the converted measuring time points, respectively.
> For example:

| ID      |     EXAMDATE | RAVLT_forgetting| day|
| :-------- | ----------:| :-------------: |----:|
| 002_S_0413| 2006-05-08 |                5|    0|
 |002_S_0413| 2006-11-14 |                4 | 190|
 |002_S_0413| 2007-06-01 |                6 | 389|
 |002_S_0413| 2008-07-31 |               10 | 815|
 |002_S_0413 |2009-04-30 |                4 |1088|
 |...        |...        |...               |... |


- genoData: a large N by q matrix with each row representing a sample; each column representing the SNP.
 
- fit0: a lmer object by fitting a base model with phenoData without any SNPs.

- Time: a character representing the name of the converted measuring time points.

- pheno: a character representing the name of the response variable in real dataset.

- method: a character representing the fitting method; can be one of the four: "lme","nebe","gallop" or "scebe".
 -- lme: data are analyzed by lmer in R/lme4
 -- nebe: naieve empirical Bayes estimation based on base model without covariates
 --gallop: GALLOP
 --scebe: our proposed two stage method
 
 ## Output

- parameter estimator for SNP effect on intercept and slope; standard error; p-values and running time


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


