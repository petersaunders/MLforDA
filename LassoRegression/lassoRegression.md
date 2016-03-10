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

I tried running the model with and without the policing data.  Including the policing data meant that I was only left with 319 instances to use.  I therefore decided it would be better to remove all of the columns with missing data.

I used a 60% training / 40% testing split with 10 fold cross-validation.

## Results

The final, optimum regression model had 

