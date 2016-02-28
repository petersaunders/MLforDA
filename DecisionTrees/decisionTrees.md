# Decision Trees

## Final Tree

- Insert Picture Here -

## SAS Code
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

## Background
I acquired data on diabetes in Pima Indians from the University of California, Irvine Machine Learning Data repository.

The dataset consists of 768 instances with 9 variates, including the class variable 'diabetes'.  These variates are:

1. pregnant - Number of times pregnant 
2. glucose - Plasma glucose concentration a 2 hours in an oral glucose tolerance test 
3. bp - Diastolic blood pressure (mm Hg) 
4. triceps - Triceps skin fold thickness (mm) 
5. insulin - 2-Hour serum insulin (mu U/ml) 
6. bmi - Body mass index (weight in kg/(height in m)^2) 
7. pedigree - Diabetes pedigree function 
8. age - Age (years) 
9. diabetes - Class variable (0 or 1)

The data contains some missing values, which are entered as 0 which is clearly not biologically possible (e.g. blood pressure, insulin, bmi).  These will need to be removed from the analysis.

## Analysis

## Results
