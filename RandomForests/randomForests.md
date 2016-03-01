# Week 2 - Random Forests

## Final Variable Importance Rankings

## SAS Code
```sas
LIBNAME mydata "/courses/d1406ae5ba27fe300 " access=readonly;

/* Import the  data from csv file */
PROC IMPORT DATAFILE='/home/petersaunders10/randomForests/mushrooms.csv' 
        OUT=imported_data REPLACE;

```

## Background
I acquired data on the edibility of mushrooms from the [University of California, Irvine Machine Learning Data repository](https://archive.ics.uci.edu/ml/datasets/Mushroom).

The dataset consists of 8124 instances with 23 variables, including the class variable of edible/poisonous.  This represents 8124 hypothetical instances of 23 different species of mushroom (not classified here).  There is no simple rule for determining the edibility of a mushroom so I want to build a random forest model which, given all the information about a particular mushroom, will predict (hopefully with a high degree of accuracy!) whether it is edible or not.

The variables are:

| Variate | Description |
| --- | --- |
|edible | edible=e, poisonous/unknown=p |
|cap-shape|                bell=b,conical=c,convex=x,flat=f,knobbed=k,sunken=s|
|cap-surface|             fibrous=f,grooves=g,scaly=y,smooth=s|
|cap-color|                brown=n,buff=b,cinnamon=c,gray=g,green=r,pink=p,purple=u,red=e,white=w,yellow=y|
|bruises|                 bruises=t,no=f|
|odor|                     almond=a,anise=l,creosote=c,fishy=y,foul=f,musty=m,none=n,pungent=p,spicy=s|
|gill-attachment|          attached=a,descending=d,free=f,notched=n|
|gill-spacing|             close=c,crowded=w,distant=d|
|gill-size|                broad=b,narrow=n|
|gill-color|               black=k,brown=n,buff=b,chocolate=h,gray=g,green=r,orange=o,pink=p,purple=u,red=e,white=w,yellow=y|
|stalk-shape|              enlarging=e,tapering=t|
|stalk-root|               bulbous=b,club=c,cup=u,equal=e,rhizomorphs=z,rooted=r,missing=?|
|stalk-surface-above-ring| fibrous=f,scaly=y,silky=k,smooth=s|
|stalk-surface-below-ring| fibrous=f,scaly=y,silky=k,smooth=s|
|stalk-color-above-ring|   brown=n,buff=b,cinnamon=c,gray=g,orange=o,pink=p,red=e,white=w,yellow=y|
|stalk-color-below-ring|   brown=n,buff=b,cinnamon=c,gray=g,orange=o,pink=p,red=e,white=w,yellow=y|
|veil-type|                partial=p,universal=u|
|veil-color|               brown=n,orange=o,white=w,yellow=y|
|ring-number|              none=n,one=o,two=t|
|ring-type|                cobwebby=c,evanescent=e,flaring=f,large=l,none=n,pendant=p,sheathing=s,zone=z|
|spore-print-color|        black=k,brown=n,buff=b,chocolate=h,green=r,orange=o,purple=u,white=w,yellow=y|
|population|               abundant=a,clustered=c,numerous=n,scattered=s,several=v,solitary=y|
|habitat|                  grasses=g,leaves=l,meadows=m,paths=p,urban=u,waste=w,woods=d|

The `stalk-root` column contains many missing data, coded using `?`, and this will need to be considered when doing the analysis.


## Analysis

I used the SAS `hpforest` procedure to build a random forest...



## Results
