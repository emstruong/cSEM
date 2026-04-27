# Summarize model

**\[stable\]**

## Usage

``` r
summarize(
 .object = NULL, 
 .alpha  = 0.05,
 .ci     = NULL,
 ...
 )
```

## Arguments

- .object:

  An R object of class
  [cSEMResults](https://floschuberth.github.io/cSEM/reference/csem_results.md)
  resulting from a call to
  [`csem()`](https://floschuberth.github.io/cSEM/reference/csem.md).

- .alpha:

  An integer or a numeric vector of significance levels. Defaults to
  `0.05`.

- .ci:

  A vector of character strings naming the confidence interval to
  compute. For possible choices see
  [`infer()`](https://floschuberth.github.io/cSEM/reference/infer.md).

- ...:

  Further arguments to `summarize()`. Currently ignored.

## Value

An object of class `cSEMSummarize`. A `cSEMSummarize` object has the
same structure as the
[cSEMResults](https://floschuberth.github.io/cSEM/reference/csem_results.md)
object with a couple differences:

1.  Elements `$Path_estimates`, `$Loadings_estimates`,
    `$Weight_estimates`, `$Weight_estimates`, and
    `$Residual_correlation` are standardized data frames instead of
    matrices.

2.  Data frames `$Effect_estimates`, `$Indicator_correlation`, and
    `$Exo_construct_correlation` are added to `$Estimates`.

The data frame format is usually much more convenient if users intend to
present the results in e.g., a paper or a presentation.

## Details

The summary is mainly focused on estimated parameters. For quality
criteria such as the average variance extracted (AVE), reliability
estimates, effect size estimates etc., use
[`assess()`](https://floschuberth.github.io/cSEM/reference/assess.md).

If `.object` contains resamples, standard errors, t-values and p-values
(assuming estimates are standard normally distributed) are printed as
well. By default the percentile confidence interval is given as well.
For other confidence intervals use the `.ci` argument. See
[`infer()`](https://floschuberth.github.io/cSEM/reference/infer.md) for
possible choices and a description.

## See also

[csem](https://floschuberth.github.io/cSEM/reference/csem.md),
[`assess()`](https://floschuberth.github.io/cSEM/reference/assess.md),
[cSEMResults](https://floschuberth.github.io/cSEM/reference/csem_results.md),
[`exportToExcel()`](https://floschuberth.github.io/cSEM/reference/exportToExcel.md)

## Examples

``` r
## Take a look at the dataset
#?threecommonfactors

## Specify the (correct) model
model <- "
# Structural model
eta2 ~ eta1
eta3 ~ eta1 + eta2

# (Reflective) measurement model
eta1 =~ y11 + y12 + y13
eta2 =~ y21 + y22 + y23
eta3 =~ y31 + y32 + y33
"

## Estimate
res <- csem(threecommonfactors, model, .resample_method = "bootstrap", .R = 40)

## Postestimation
res_summarize <- summarize(res)
res_summarize
#> ________________________________________________________________________________
#> ----------------------------------- Overview -----------------------------------
#> 
#>  General information:
#>  ------------------------
#>  Estimation status                  = Ok
#>  Number of observations             = 500
#>  Weight estimator                   = PLS-PM
#>  Inner weighting scheme             = "path"
#>  Type of indicator correlation      = Pearson
#>  Path model estimator               = OLS
#>  Second-order approach              = NA
#>  Type of path model                 = Linear
#>  Disattenuated                      = Yes (PLSc)
#> 
#>  Resample information:
#>  ---------------------
#>  Resample method                    = "bootstrap"
#>  Number of resamples                = 40
#>  Number of admissible results       = 40
#>  Approach to handle inadmissibles   = "drop"
#>  Sign change option                 = "none"
#>  Random seed                        = -1093679242
#> 
#>  Construct details:
#>  ------------------
#>  Name  Modeled as     Order         Mode      
#> 
#>  eta1  Common factor  First order   "modeA"   
#>  eta2  Common factor  First order   "modeA"   
#>  eta3  Common factor  First order   "modeA"   
#> 
#> ----------------------------------- Estimates ----------------------------------
#> 
#> Estimated path coefficients:
#> ============================
#>                                                              CI_percentile   
#>   Path           Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1      0.6713      0.0397   16.9218    0.0000 [ 0.6020; 0.7467 ] 
#>   eta3 ~ eta1      0.4585      0.0987    4.6471    0.0000 [ 0.2310; 0.5750 ] 
#>   eta3 ~ eta2      0.3052      0.1007    3.0318    0.0024 [ 0.1544; 0.5051 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_percentile   
#>   Loading        Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 =~ y11      0.6631      0.0459   14.4579    0.0000 [ 0.5794; 0.7425 ] 
#>   eta1 =~ y12      0.6493      0.0454   14.3169    0.0000 [ 0.5608; 0.7116 ] 
#>   eta1 =~ y13      0.7613      0.0332   22.9338    0.0000 [ 0.7010; 0.8146 ] 
#>   eta2 =~ y21      0.5165      0.0591    8.7434    0.0000 [ 0.3961; 0.6251 ] 
#>   eta2 =~ y22      0.7554      0.0424   17.8227    0.0000 [ 0.6663; 0.8212 ] 
#>   eta2 =~ y23      0.7997      0.0364   21.9647    0.0000 [ 0.7312; 0.8673 ] 
#>   eta3 =~ y31      0.8223      0.0262   31.4240    0.0000 [ 0.7693; 0.8514 ] 
#>   eta3 =~ y32      0.6581      0.0414   15.8922    0.0000 [ 0.5889; 0.7394 ] 
#>   eta3 =~ y33      0.7474      0.0412   18.1428    0.0000 [ 0.6617; 0.8174 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_percentile   
#>   Weight         Estimate  Std. error   t-stat.   p-value         95%        
#>   eta1 <~ y11      0.3956      0.0249   15.9064    0.0000 [ 0.3632; 0.4417 ] 
#>   eta1 <~ y12      0.3873      0.0203   19.0389    0.0000 [ 0.3576; 0.4264 ] 
#>   eta1 <~ y13      0.4542      0.0240   18.9563    0.0000 [ 0.4078; 0.4956 ] 
#>   eta2 <~ y21      0.3058      0.0313    9.7701    0.0000 [ 0.2481; 0.3706 ] 
#>   eta2 <~ y22      0.4473      0.0261   17.1464    0.0000 [ 0.4105; 0.4929 ] 
#>   eta2 <~ y23      0.4735      0.0212   22.2853    0.0000 [ 0.4235; 0.5024 ] 
#>   eta3 <~ y31      0.4400      0.0183   24.0254    0.0000 [ 0.4046; 0.4661 ] 
#>   eta3 <~ y32      0.3521      0.0197   17.8557    0.0000 [ 0.3228; 0.3913 ] 
#>   eta3 <~ y33      0.3999      0.0182   21.9987    0.0000 [ 0.3571; 0.4342 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_percentile   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta2 ~ eta1       0.6713      0.0397   16.9218    0.0000 [ 0.6020; 0.7467 ] 
#>   eta3 ~ eta1       0.6634      0.0408   16.2728    0.0000 [ 0.5820; 0.7222 ] 
#>   eta3 ~ eta2       0.3052      0.1007    3.0318    0.0024 [ 0.1544; 0.5051 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_percentile   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         95%        
#>   eta3 ~ eta1          0.2049      0.0701    2.9217    0.0035 [ 0.0969; 0.3508 ] 
#> ________________________________________________________________________________

# Extract e.g. the loadings
res_summarize$Estimates$Loading_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat       p_value
#> 1 eta1 =~ y11  Common factor 0.6630699 0.04586223 14.457865  2.236278e-47
#> 2 eta1 =~ y12  Common factor 0.6492779 0.04535046 14.316899  1.716110e-46
#> 3 eta1 =~ y13  Common factor 0.7613458 0.03319753 22.933810 2.137743e-116
#> 4 eta2 =~ y21  Common factor 0.5164548 0.05906819  8.743366  2.262667e-18
#> 5 eta2 =~ y22  Common factor 0.7553877 0.04238349 17.822688  4.711720e-71
#> 6 eta2 =~ y23  Common factor 0.7996637 0.03640679 21.964684 6.269135e-107
#> 7 eta3 =~ y31  Common factor 0.8222773 0.02616721 31.423962 9.524791e-217
#> 8 eta3 =~ y32  Common factor 0.6580689 0.04140835 15.892178  7.178916e-57
#> 9 eta3 =~ y33  Common factor 0.7474241 0.04119676 18.142789  1.464098e-73
#>   CI_percentile.95%L CI_percentile.95%U
#> 1          0.5793623          0.7425279
#> 2          0.5607810          0.7116365
#> 3          0.7009549          0.8146072
#> 4          0.3960574          0.6251259
#> 5          0.6663291          0.8211956
#> 6          0.7311857          0.8672691
#> 7          0.7693415          0.8513699
#> 8          0.5888729          0.7393735
#> 9          0.6617387          0.8174394

## By default only the 95% percentile confidence interval is printed. User
## can have several confidence interval computed, however, only the first
## will be printed.

res_summarize <- summarize(res, .ci = c("CI_standard_t", "CI_percentile"), 
                           .alpha = c(0.05, 0.01))
res_summarize
#> ________________________________________________________________________________
#> ----------------------------------- Overview -----------------------------------
#> 
#>  General information:
#>  ------------------------
#>  Estimation status                  = Ok
#>  Number of observations             = 500
#>  Weight estimator                   = PLS-PM
#>  Inner weighting scheme             = "path"
#>  Type of indicator correlation      = Pearson
#>  Path model estimator               = OLS
#>  Second-order approach              = NA
#>  Type of path model                 = Linear
#>  Disattenuated                      = Yes (PLSc)
#> 
#>  Resample information:
#>  ---------------------
#>  Resample method                    = "bootstrap"
#>  Number of resamples                = 40
#>  Number of admissible results       = 40
#>  Approach to handle inadmissibles   = "drop"
#>  Sign change option                 = "none"
#>  Random seed                        = -1093679242
#> 
#>  Construct details:
#>  ------------------
#>  Name  Modeled as     Order         Mode      
#> 
#>  eta1  Common factor  First order   "modeA"   
#>  eta2  Common factor  First order   "modeA"   
#>  eta3  Common factor  First order   "modeA"   
#> 
#> ----------------------------------- Estimates ----------------------------------By default, only one confidence interval supplied to `.ci` is printed.
#> Use `xxx` to print all confidence intervals (not yet implemented).
#> 
#> 
#> 
#> Estimated path coefficients:
#> ============================
#>                                                              CI_standard_t   
#>   Path           Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1      0.6713      0.0397   16.9218    0.0000 [ 0.5707; 0.7759 ] 
#>   eta3 ~ eta1      0.4585      0.0987    4.6471    0.0000 [ 0.2121; 0.7224 ] 
#>   eta3 ~ eta2      0.3052      0.1007    3.0318    0.0024 [ 0.0287; 0.5492 ] 
#> 
#> Estimated loadings:
#> ===================
#>                                                              CI_standard_t   
#>   Loading        Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 =~ y11      0.6631      0.0459   14.4579    0.0000 [ 0.5396; 0.7768 ] 
#>   eta1 =~ y12      0.6493      0.0454   14.3169    0.0000 [ 0.5390; 0.7735 ] 
#>   eta1 =~ y13      0.7613      0.0332   22.9338    0.0000 [ 0.6867; 0.8583 ] 
#>   eta2 =~ y21      0.5165      0.0591    8.7434    0.0000 [ 0.3666; 0.6721 ] 
#>   eta2 =~ y22      0.7554      0.0424   17.8227    0.0000 [ 0.6501; 0.8692 ] 
#>   eta2 =~ y23      0.7997      0.0364   21.9647    0.0000 [ 0.7135; 0.9018 ] 
#>   eta3 =~ y31      0.8223      0.0262   31.4240    0.0000 [ 0.7644; 0.8997 ] 
#>   eta3 =~ y32      0.6581      0.0414   15.8922    0.0000 [ 0.5486; 0.7627 ] 
#>   eta3 =~ y33      0.7474      0.0412   18.1428    0.0000 [ 0.6458; 0.8589 ] 
#> 
#> Estimated weights:
#> ==================
#>                                                              CI_standard_t   
#>   Weight         Estimate  Std. error   t-stat.   p-value         99%        
#>   eta1 <~ y11      0.3956      0.0249   15.9064    0.0000 [ 0.3248; 0.4534 ] 
#>   eta1 <~ y12      0.3873      0.0203   19.0389    0.0000 [ 0.3357; 0.4409 ] 
#>   eta1 <~ y13      0.4542      0.0240   18.9563    0.0000 [ 0.3945; 0.5184 ] 
#>   eta2 <~ y21      0.3058      0.0313    9.7701    0.0000 [ 0.2240; 0.3859 ] 
#>   eta2 <~ y22      0.4473      0.0261   17.1464    0.0000 [ 0.3779; 0.5128 ] 
#>   eta2 <~ y23      0.4735      0.0212   22.2853    0.0000 [ 0.4186; 0.5285 ] 
#>   eta3 <~ y31      0.4400      0.0183   24.0254    0.0000 [ 0.3941; 0.4888 ] 
#>   eta3 <~ y32      0.3521      0.0197   17.8557    0.0000 [ 0.2970; 0.3990 ] 
#>   eta3 <~ y33      0.3999      0.0182   21.9987    0.0000 [ 0.3524; 0.4464 ] 
#> 
#> ------------------------------------ Effects -----------------------------------
#> 
#> Estimated total effects:
#> ========================
#>                                                               CI_standard_t   
#>   Total effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta2 ~ eta1       0.6713      0.0397   16.9218    0.0000 [ 0.5707; 0.7759 ] 
#>   eta3 ~ eta1       0.6634      0.0408   16.2728    0.0000 [ 0.5560; 0.7668 ] 
#>   eta3 ~ eta2       0.3052      0.1007    3.0318    0.0024 [ 0.0287; 0.5492 ] 
#> 
#> Estimated indirect effects:
#> ===========================
#>                                                                  CI_standard_t   
#>   Indirect effect    Estimate  Std. error   t-stat.   p-value         99%        
#>   eta3 ~ eta1          0.2049      0.0701    2.9217    0.0035 [ 0.0129; 0.3754 ] 
#> ________________________________________________________________________________

# Extract the loading including both confidence intervals
res_summarize$Estimates$Path_estimates
#>          Name Construct_type  Estimate    Std_err    t_stat      p_value
#> 1 eta2 ~ eta1  Common factor 0.6713334 0.03967279 16.921758 3.110049e-64
#> 2 eta3 ~ eta1  Common factor 0.4585068 0.09866508  4.647103 3.366291e-06
#> 3 eta3 ~ eta2  Common factor 0.3051511 0.10065160  3.031756 2.431354e-03
#>   CI_standard_t.99%L CI_standard_t.99%U CI_standard_t.95%L CI_standard_t.95%U
#> 1          0.5707183          0.7758836         0.59535466          0.7512473
#> 2          0.2121211          0.7223611         0.27339092          0.6610913
#> 3          0.0287184          0.5492316         0.09122186          0.4867282
#>   CI_percentile.99%L CI_percentile.99%U CI_percentile.95%L CI_percentile.95%U
#> 1          0.5805329          0.7518850          0.6020392          0.7467094
#> 2          0.2238610          0.6337014          0.2309842          0.5749635
#> 3          0.1184126          0.5106955          0.1543827          0.5050775
```
