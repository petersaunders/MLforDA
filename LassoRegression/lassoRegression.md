# Week 3 - Lasso Regression

## Final Result

## SAS Code
```sas
LIBNAME mydata "/courses/d1406ae5ba27fe300 " access=readonly;

/* Import the crime data from csv file */
PROC IMPORT DATAFILE='/home/petersaunders10/lassoRegression/crime.csv' 
        OUT=imported_data REPLACE;
        

/* Split data into training and testing sets */
PROC SURVEYSELECT data=imported_data out=traintest seed=1234 samprate=0.6 method=srs outall;
RUN;   

ods graphics on;

/* Lasso Regression with selected Explanatory Variables */
PROC GLMSELECT data=traintest plots=all seed=5678;
     partition ROLE=selected(train='1' test='0');
     MODEL ViolentCrimesPerPop=population householdsize racepctblack racePctWhite 
     racePctAsian racePctHisp agePct12t21 agePct12t29 agePct16t24 agePct65up
     numbUrban pctUrban medIncome pctWWage pctWFarmSelf pctWInvInc pctWSocSec
     pctWPubAsst pctWRetire medFamInc perCapInc whitePerCap blackPerCap
     indianPerCap AsianPerCap OtherPerCap HispPerCap NumUnderPov PctPopUnderPov
     PctLess9thGrade PctNotHSGrad PctBSorMore PctUnemployed PctEmploy
     PctEmplManu PctEmplProfServ PctOccupManu PctOccupMgmtProf MalePctDivorce
     MalePctNevMarr FemalePctDiv TotalPctDiv PersPerFam PctFam2Par PctKids2Par
     PctYoungKids2Par PctTeen2Par PctWorkMomYoungKids PctWorkMom NumIlleg
     PctIlleg NumImmig PctImmigRecent PctImmigRec5 PctImmigRec8 PctImmigRec10
     PctRecentImmig PctRecImmig5 PctRecImmig8 PctRecImmig10 PctSpeakEnglOnly
     PctNotSpeakEnglWell PctLargHouseFam PctLargHouseOccup PersPerOccupHous
     PersPerOwnOccHous PersPerRentOccHous PctPersOwnOccup PctPersDenseHous
     PctHousLess3BR MedNumBR HousVacant PctHousOccup PctHousOwnOcc 
     PctVacantBoarded PctVacMore6Mos MedYrHousBuilt PctHousNoPhone
     PctWOFullPlumb OwnOccLowQuart OwnOccMedVal OwnOccHiQuart RentLowQ RentMedian
     RentHighQ MedRent MedRentPctHousInc MedOwnCostPctInc MedOwnCostPctIncNoMtg
     NumInShelters NumStreet PctForeignBorn PctBornSameState PctSameHouse85
     PctSameCity85 PctSameState85 LandArea PopDens PctUsePubTrans 
     LemasPctOfficDrugUn/selection=lar(choose=cv stop=none) cvmethod=random(10);
RUN;
```

## Background
I acquired data on violent crime in US cities from the [University of California, Irvine Machine Learning Data repository](https://archive.ics.uci.edu/ml/datasets/Communities+and+Crime).  This dataset contains very large pool of 122 predictor variable (as opposed to some purely instance identifying varibles, such as `state`, `county` and `communityname`) in addition to the response variable `ViolentCrimesPerPop`.

The predictors are a collection of statistics for the community including race/ethnicity, employment rate, housing quality and family composition.  Some instances also have statistics relating to the local policing, however these are only available for around 15% of the communities covered.

It would be excessive to list all of the variables here, so, if you are interested in the full list, see the link above.  I will mention only the variables which appear in the final lasso regression model.

Some of the predictor variables in the dataset are simply related to others, such as the variable `PopDens`, the population density, which is just the variable `population` divided by the variable `LandArea`.  We should expect the lasso regression to select from these highly correlated variables.

Having so many explanatory variables should be a good test of the lasso regression method and how effective it is at identifying which variables are important out of the many available.

## Analysis

The data-set already contains a pre-defined `fold` variable which is set up for *non-random* 10 fold cross-valdation.  This variable is only intended to be used for debugging and I will not be using it for building my model.  Instead I used randomly generated folds.

I tried running the model with and without the policing data.  Including the policing data meant that I was only left with 319 instances to use.  I therefore decided it would be better to remove all of the columns with missing data.  This then left me with exactly 100 predictor variables.

I used a 60% training / 40% testing split with 10 fold cross-validation.  With the full data-set this meant that 1197 instances were used for training and 796 for testing.

![GLM Select Details](/images/glmSelect.png)

## Results

The final, selected, optimum regression model included 24 out of 100 possible variables.  The variable selection table is shown below, with the optimum model highlighted.

![Top Variables](/images/variableSelect.png)

The model selection curves were:

![Model Selection Curves](/images/modelCurves.png)

The top 4 variables are substantially more important than the rest.  This is somewhat reflected in the regression coefficients which, since the data is normalised, are an approximate measure of the relative importance of the covariates.  It is also important to note the signs of the coefficients.

| Covariate    | Coeff.  | Description                                          |
|==============|=========|======================================================|
| PctIlleg     | +0.186  | percentage of kids born to never married             |
| PctKids2Par  | -0.289  | percentage of kids in family housing with two parents|
| racePctWhite | -0.053  | percentage of population that is caucasian           |
| NumIlleg     | +0.041  | number of kids born to never married                 |
| HousVacant   | +0.169  | number of vacant households                          |

We can see from the top coefficients that family, race and housing conditions are all important factors in modelling the rate of violent crime.  These might be proxies for some other (unreported or unobtainable) statistic such as the poverty rate or the prevalence of gang culture.

