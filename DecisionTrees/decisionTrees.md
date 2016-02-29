# Week 1 - Decision Trees

## Final Decision Tree

![Diabetes Decision Tree](images/tree.png?raw=true)

## SAS Code
```sas
LIBNAME mydata "/courses/d1406ae5ba27fe300 " access=readonly;

/* Import the diabetes data from csv file */
PROC IMPORT DATAFILE='/home/petersaunders10/decisionTrees/pima-indians-diabetes.csv' 
        OUT=imported_data REPLACE;
        
/* Filter to remove missing (0) data */
DATA diabetes_data;
    SET imported_data;
    IF bp>0 AND triceps>0 AND bmi>0 AND insulin>0;

/* Build the decision tree */
ods graphics on;

proc hpsplit data=diabetes_data seed=1234;
    class diabetes;
    model diabetes = pregnant glucose bp triceps insulin bmi pedigree age;
    criterion entropy;
    prune cc;    
RUN;
```

## Background
I acquired data on diabetes in Pima Indians from the [University of California, Irvine Machine Learning Data repository](https://archive.ics.uci.edu/ml/datasets/Pima+Indians+Diabetes).

The dataset consists of 768 instances with 9 variates, including the class variable 'diabetes'.  These variates are:

| Variate | Description |
| --- | --- |
| pregnant | Number of times pregnant |
| glucose | Plasma glucose concentration a 2 hours in an oral glucose tolerance test |
| bp | Diastolic blood pressure (mm Hg)  |
| triceps | Triceps skin fold thickness (mm) |
| insulin | 2-Hour serum insulin (mu U/ml)  |
| bmi | Body mass index (weight in kg/(height in m)^2)  |
| pedigree | Diabetes pedigree function  |
| age | Age (years)  |
| diabetes | Class variable (0 or 1) |

The data contains some missing values, entered as 0, which are clearly not biologically possible (e.g. blood pressure, insulin, bmi).  These will need to be removed from the analysis.

## Analysis

I used the SAS `hpsplit` procedure to produce a decision tree for the response variable `diabetes` using the other 8 pieces of data as continuous variables.  After filtering out the invalid data I was left with 392 instances on which to build the model.

Using cost-complexity pruning produced a tree with 4 leaves which uses just the covariates `glucose` and `age`.

## Results

The first split is on blood glucose level, whether the individual has a blood glucose level above or below 127.  If the blood glucose level is below 127 they are classified as not having diabetes.  This prediction is correct for 85% of the training instances.

If the individual has blood glucose level above 127 then a further split is done on blood glucose level at a threshold of 165.3.  If their glucose level is above 165.3 then they are classified as having diabetes.  This prediction is correct for 89% of the training cases.

If their glucose level is below 165.3 then further split is done on the `age` variable at a threshold of 23.4.  If the individual is below 23.4 they are predicted not to have diabetes, and otherwise predicted to have diabetes.  The negative prediction is correct in 95% of the training cases and the positive prediction 59%.

These results are summarised in the confusion matrix:

|        |   | Predicted |            ||
| ------ |---|-----|-----|------------|
|        |   | **0**  | **1**  | **Error Rate** |
| **Actual** | **0** | 221 | 41  | 16%        |
|            | **1** | 37  | 93  | 28%        |
