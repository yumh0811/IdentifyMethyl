# IdentifyMethyl

This package provides a Pearson nearest-centroid classification intended for assigning class labels to single samples according to the three BCPP methylation subgroups of non-muscle-invasive bladder cancer (NMIBC): M1, M2 and M3. The three BCPP methylation subgroups are achieved by applying the pam algorithm to methylation beta value of 2,184 CpGs from Infinium HumanMethylation450 BeadChip (HM450K).


## Citation 

Please cite xxx. DOI: xxx


## Install

You may install this package with devtools:
``` {r}
library(devtools)

devtools::install_github("yumh0811/IdentifyMethyl", build_vignettes = TRUE)

library(IdentifyMethyl)
```

## Usage

``` {r}
IdentifyMethyl(x, minCor = .2)
```
`x`:  data.frame with CpGs from HM450K or Infinium MethylationEPIC (Epic) in rows and samples to be classified in columns (accepting a dataframe with a single column namely one sample).

`minCor`: numeric value specifying a minimal threshold for best Pearson's correlation between a sample's methylation profile and centroids profiles. A sample showing no correlation above this threshold will remain unclassifed and prediction results will be set to NA. Default minCor value is 0.2.


## Example

``` {r}
data(test_data)

Methylation_subgroups <- IdentifyMethyl(test_data, minCor = .2)

head(Methylation_subgroups)
```
|         | Methylation_subgroups | cor_pval | separationLevel | M1          | M2         | M3         |
| ------- | --------------------- | -------- | --------------- | ----------- | ---------- | ---------- |
| sample1 | M1                    | 0        | 1               | 0.74934091  | -0.2140584 | -0.2067673 |
| sample2 | M2                    | 0        | 1               | -0.12962299 | 0.8082984  | -0.3620718 |
| sample3 | M3                    | 0        | 1               | 0.08225613  | -0.6091523 | 0.8532240  |

The classifier returns a data.frame with 6 columns:

`Methylation_subgroups`: the predicted class labels of the samples.

`cor_pval`: the p-value associated with the Pearson's correlation between the sample and the nearest centroid.

`separationLevel`: gives a measure (ranging from 0 to 1) of how a sample is representative of its consensus class, with 0 meaning the sample is too close to the other classes to be confidently assigned to one class label, and 1 meaning the sample is very representative of its class. It is measured as follows: (correlation to nearest centroid - correlation to second nearest centroid) / median difference of sample-to-centroid correlation.

`M1` `M2` `M3`: The remaining three columns return the Pearson's correlation values for each sample and each centroid. 
