# Week 3 - Lasso Regression

## Final Result

## SAS Code
```sas
LIBNAME mydata "/courses/d1406ae5ba27fe300 " access=readonly;

/* Put SAS code here */

```

## Background
I acquired data on violent crime in US cities from the [University of California, Irvine Machine Learning Data repository](https://archive.ics.uci.edu/ml/datasets/Communities+and+Crime).  This dataset contains very large pool of 122 predictor variable (as opposed to some purely instance identifying varibles, such as `state`, `county` and `communityname`) in addition to the response variable `ViolentCrimesPerPop`.

It would be excessive to list all of the variables here, so, if you are interested in the full list, see the link above.  I will mention only the variables which appear in the final lasso regression model (and perhaps those which are notable by their absence).

Some of the predictor variables in the dataset are simply related to others, such as the variable `PopDens`, the population density, which is just the variable `population` divided by the variable `LandArea`.  We should expect the lasso regression to select from these highly correlated variables.

Having so many explanatory variables should be a good test of the lasso regression method and how effective it is at identifying which variables are important out of the many available.

## Analysis

The data-set already contains a pre-defined `fold` variable which is set up

## Results
