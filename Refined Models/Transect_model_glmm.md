Transect\_glmm
================
LRA
1/26/2021

## Transect level models of disease

Two sets of models, one with cumulative positive temperature anomaly as
a predictor, one without.

Because only 27 meadows out of 32 have SST data for the anomaly
calculation, the number of replicates is smaller for those models.

However, the significant effects are mainly the same across the models.

Predictors are: Blade Area, Shoot Density, Epiphyte Mass per Blade Area,
Cumulative positive anomaly, Tidal Height, and all interactions between
Tidal Height and fixed effects.

Random structure for transects is Meadow within Region.

Note, for severity, transect-level severity is re-summarized based on
diseased blades only. Predictor values are the same.

``` r
transect <- read.csv("Refined Models/input/transect_combined_parameters.csv")
transect$Region <- ordered(transect$Region,levels=region_order)
transect$Meadow <- as.factor(transect$Meadow)
transect$sBladeAreaMean <- scale(transect$BladeAreaMean,scale=TRUE,center=TRUE)
transect$sDensityShootsMean <- scale(transect$DensityShootsMean,scale=TRUE,center=TRUE)
transect$sEpiphytePerAreaMean <- scale(transect$EpiphytePerAreaMean,scale=TRUE,center=TRUE)
transect$sCDiffMeanHeat <- scale(transect$CDiffMeanHeat,scale=TRUE,center=TRUE)
dat <- transect[-which(is.na((transect$CDiffMeanHeat))),]
dat$Region <- ordered(dat$Region,levels=region_order)
dat$sBladeAreaMean <- scale(dat$BladeAreaMean,scale=TRUE,center=TRUE)
dat$sDensityShootsMean <- scale(dat$DensityShootsMean,scale=TRUE,center=TRUE)
dat$sEpiphytePerAreaMean <- scale(dat$EpiphytePerAreaMean,scale=TRUE,center=TRUE)
dat$sCDiffMeanHeat <- scale(dat$CDiffMeanHeat,scale=TRUE,center=TRUE)

# data exploration
# transect_dat <- select(transect,c("TidalHeight","LongestBladeLengthMean","EpiphytePerAreaMean","CDiffMeanHeat",
#                                   "DensityShootsMean","BladeAreaMean","PrevalenceMean","SeverityMean"))
# transect_dat$TidalHeight <- as.factor(transect_dat$TidalHeight)
# pairs(transect_dat)
```

## Prevalence model with CPTA n=162

predictors: Density, Blade Area, Epiphyte mass per area, CPTA, Tidal
height, and interactions between fixed effects and TH random structure:
site within region

    ##  Family: binomial  ( logit )
    ## Formula:          
    ## PrevalenceMean ~ sBladeAreaMean + sDensityShootsMean + sEpiphytePerAreaMean +  
    ##     sCDiffMeanHeat + TidalHeight + sDensityShootsMean:TidalHeight +  
    ##     sEpiphytePerAreaMean:TidalHeight + sCDiffMeanHeat:TidalHeight +  
    ##     (1 | Meadow)
    ## Data: dat
    ## Weights: CountBlades
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##    988.3   1019.2   -484.1    968.3      152 
    ## 
    ## Random effects:
    ## 
    ## Conditional model:
    ##  Groups Name        Variance Std.Dev.
    ##  Meadow (Intercept) 1.186    1.089   
    ## Number of obs: 162, groups:  Meadow, 27
    ## 
    ## Conditional model:
    ##                                   Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)                        0.15711    0.22416   0.701  0.48339    
    ## sBladeAreaMean                    -0.11612    0.10800  -1.075  0.28230    
    ## sDensityShootsMean                 0.49850    0.22171   2.248  0.02455 *  
    ## sEpiphytePerAreaMean               0.25640    0.15270   1.679  0.09314 .  
    ## sCDiffMeanHeat                     0.90115    0.22518   4.002 6.28e-05 ***
    ## TidalHeightU                      -0.10492    0.11235  -0.934  0.35035    
    ## sDensityShootsMean:TidalHeightU   -0.55585    0.19374  -2.869  0.00412 ** 
    ## sEpiphytePerAreaMean:TidalHeightU -0.28188    0.14722  -1.915  0.05554 .  
    ## sCDiffMeanHeat:TidalHeightU       -0.25657    0.09629  -2.664  0.00771 ** 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    ## Warning in stats::dbinom(x, size = n, prob = mean): NaNs produced

    ## # Indices of model performance
    ## 
    ## AIC    |     BIC | R2 (cond.) | R2 (marg.) |  ICC | RMSE | Sigma | Log_loss
    ## ---------------------------------------------------------------------------
    ## 988.30 | 1019.17 |       0.36 |       0.13 | 0.26 | 0.15 |  1.00 |     0.13

    ## # ICC by Group
    ## 
    ## Group  |   ICC
    ## --------------
    ## Meadow | 0.265

## Transect-level prevalence model, for all transects, no CPTA, n=192

``` r
transect$Meadow <- paste(transect$Region,transect$SiteCode,sep="_")
fit_prev5 <- glmmTMB(PrevalenceMean~sBladeAreaMean+sDensityShootsMean+sEpiphytePerAreaMean+TidalHeight+
                       sBladeAreaMean:TidalHeight+sDensityShootsMean:TidalHeight+sEpiphytePerAreaMean:TidalHeight+
                       +(1|Region)+(1|Meadow),
                     data=transect,
                     weights=CountBlades,
                     family=binomial)

summary(fit_prev5)
```

    ##  Family: binomial  ( logit )
    ## Formula:          
    ## PrevalenceMean ~ sBladeAreaMean + sDensityShootsMean + sEpiphytePerAreaMean +  
    ##     TidalHeight + sBladeAreaMean:TidalHeight + sDensityShootsMean:TidalHeight +  
    ##     sEpiphytePerAreaMean:TidalHeight + +(1 | Region) + (1 | Meadow)
    ## Data: transect
    ## Weights: CountBlades
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##   1150.2   1182.8   -565.1   1130.2      182 
    ## 
    ## Random effects:
    ## 
    ## Conditional model:
    ##  Groups Name        Variance Std.Dev.
    ##  Region (Intercept) 0.6434   0.8021  
    ##  Meadow (Intercept) 1.1228   1.0596  
    ## Number of obs: 192, groups:  Region, 6; Meadow, 32
    ## 
    ## Conditional model:
    ##                                   Estimate Std. Error z value Pr(>|z|)   
    ## (Intercept)                         0.1801     0.3851   0.468  0.64005   
    ## sBladeAreaMean                     -0.2494     0.1076  -2.317  0.02049 * 
    ## sDensityShootsMean                  0.4174     0.2071   2.015  0.04385 * 
    ## sEpiphytePerAreaMean                0.2971     0.1287   2.308  0.02102 * 
    ## TidalHeightU                       -0.1448     0.1240  -1.167  0.24306   
    ## sBladeAreaMean:TidalHeightU         0.1744     0.1309   1.332  0.18281   
    ## sDensityShootsMean:TidalHeightU    -0.4244     0.1850  -2.294  0.02179 * 
    ## sEpiphytePerAreaMean:TidalHeightU  -0.3265     0.1223  -2.669  0.00761 **
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
performance(fit_prev5)
```

    ## Warning in stats::dbinom(x, size = n, prob = mean): NaNs produced

    ## # Indices of model performance
    ## 
    ## AIC     |     BIC | R2 (cond.) | R2 (marg.) |  ICC | RMSE | Sigma | Log_loss
    ## ----------------------------------------------------------------------------
    ## 1150.21 | 1182.78 |       0.36 |       0.02 | 0.35 | 0.15 |  1.00 |     0.13

``` r
icc(fit_prev5,by_group = TRUE)
```

    ## # ICC by Group
    ## 
    ## Group  |   ICC
    ## --------------
    ## Region | 0.127
    ## Meadow | 0.222

``` r
#drop1(fit_prev5)
```

## transect level severity with CPTA n = 155

``` r
sev <- read.csv("Refined Models/input/severity_blades.csv")
sevT <- sev %>%
  group_by(Region,SiteCode,TidalHeight,Transect)%>%
  summarise(across(c("Severity","BladeArea","DensityShoots","EpiphytePerAreaMean","CDiffMeanHeat",
                     "sBladeArea","sDensityShoots","sEpiphytePerAreaMean","sCDiffMeanHeat"),mean))
```

    ## `summarise()` has grouped output by 'Region', 'SiteCode', 'TidalHeight'. You can override using the `.groups` argument.

``` r
sevT$MeadowId <- paste(sevT$Region,sevT$SiteCode,sep="_")
sevT$TransectId <- paste(sevT$MeadowId,sevT$Transect,sep="_")

sevTdat <- sevT[-which(is.na((sevT$CDiffMeanHeat))),]
sevTdat$sBladeAreaMean <- scale(sevTdat$BladeArea,scale=TRUE,center=TRUE)
sevTdat$sDensityShootsMean <- scale(sevTdat$DensityShoots,scale=TRUE,center=TRUE)
sevTdat$sEpiphytePerAreaMean <- scale(sevTdat$EpiphytePerAreaMean,scale=TRUE,center=TRUE)
sevTdat$sCDiffMeanHeat <- scale(sevTdat$CDiffMeanHeat,scale=TRUE,center=TRUE)

## fit with CPTA to start (data = sevTdat)
# fit_sev1 <- glmmTMB(Severity~sBladeArea+sDensityShoots+sEpiphytePerAreaMean+sCDiffMeanHeat+TidalHeight+
#                       sBladeArea:TidalHeight+sDensityShoots:TidalHeight+sEpiphytePerAreaMean:TidalHeight+sCDiffMeanHeat:TidalHeight+
#                       (1|Region)+(1|MeadowId),
#                     data=sevTdat,
#                     family=beta_family(link = "logit"))
# summary(fit_sev1)
# # again region variance is wonky because of unevenness
# fit_sev2 <- glmmTMB(Severity~sBladeArea+sDensityShoots+sEpiphytePerAreaMean+sCDiffMeanHeat+TidalHeight+
#                       sBladeArea:TidalHeight+sDensityShoots:TidalHeight+sEpiphytePerAreaMean:TidalHeight+sCDiffMeanHeat:TidalHeight+
#                       Region+(1|MeadowId),
#                     data=sevTdat,
#                     family=beta_family(link = "logit"))
# summary(fit_sev2)
# drop1(fit_sev2)
# drop Region
fit_sev3 <- glmmTMB(Severity~sBladeArea+sDensityShoots+sEpiphytePerAreaMean+sCDiffMeanHeat+TidalHeight+
                      sBladeArea:TidalHeight+sDensityShoots:TidalHeight+sEpiphytePerAreaMean:TidalHeight+sCDiffMeanHeat:TidalHeight+
                      (1|MeadowId),
                    data=sevTdat,
                    family=beta_family(link = "logit"))
summary(fit_sev3)
```

    ##  Family: beta  ( logit )
    ## Formula:          
    ## Severity ~ sBladeArea + sDensityShoots + sEpiphytePerAreaMean +  
    ##     sCDiffMeanHeat + TidalHeight + sBladeArea:TidalHeight + sDensityShoots:TidalHeight +  
    ##     sEpiphytePerAreaMean:TidalHeight + sCDiffMeanHeat:TidalHeight +  
    ##     (1 | MeadowId)
    ## Data: sevTdat
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##   -496.1   -459.6    260.0   -520.1      143 
    ## 
    ## Random effects:
    ## 
    ## Conditional model:
    ##  Groups   Name        Variance Std.Dev.
    ##  MeadowId (Intercept) 0.2765   0.5259  
    ## Number of obs: 155, groups:  MeadowId, 27
    ## 
    ## Overdispersion parameter for beta family (): 25.2 
    ## 
    ## Conditional model:
    ##                                   Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)                       -2.44868    0.13740 -17.822  < 2e-16 ***
    ## sBladeArea                        -0.36330    0.11341  -3.203  0.00136 ** 
    ## sDensityShoots                     0.51853    0.18544   2.796  0.00517 ** 
    ## sEpiphytePerAreaMean               0.21383    0.14953   1.430  0.15273    
    ## sCDiffMeanHeat                     0.10410    0.13199   0.789  0.43030    
    ## TidalHeightU                      -0.13705    0.12990  -1.055  0.29138    
    ## sBladeArea:TidalHeightU           -0.09552    0.16796  -0.569  0.56955    
    ## sDensityShoots:TidalHeightU       -0.40649    0.17351  -2.343  0.01914 *  
    ## sEpiphytePerAreaMean:TidalHeightU -0.16047    0.15213  -1.055  0.29151    
    ## sCDiffMeanHeat:TidalHeightU        0.08890    0.11053   0.804  0.42121    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
# drop1(fit_sev3)
# fit_sev4 <- glmmTMB(Severity~sBladeArea+sDensityShoots+sEpiphytePerAreaMean+sCDiffMeanHeat+TidalHeight+
#                       sBladeArea:TidalHeight+sDensityShoots:TidalHeight+sEpiphytePerAreaMean:TidalHeight+
#                       (1|MeadowId),
#                     data=sevTdat,
#                     family=beta_family(link = "logit"))
# anova(fit_sev4,fit_sev3)
# # no difference by AIC but log-likelihood test suggests we don't need it 
# fit_sev5 <- glmmTMB(Severity~sBladeArea+sDensityShoots+sEpiphytePerAreaMean+sCDiffMeanHeat+TidalHeight+
#                       sBladeArea:TidalHeight+sDensityShoots:TidalHeight+
#                       (1|MeadowId),
#                     data=sevTdat,
#                     family=beta_family(link = "logit"))
# anova(fit_sev5,fit_sev4)
# also can drop the interactions?
# fit_sev6 <- glmmTMB(Severity~sBladeArea+sDensityShoots+sEpiphytePerAreaMean+sCDiffMeanHeat+TidalHeight+
#                       sDensityShoots:TidalHeight+
#                       (1|MeadowId),
#                     data=sevTdat,
#                     family=beta_family(link = "logit"))
# anova(fit_sev6,fit_sev5)
# drop1(fit_sev6)
# summary(fit_sev6)
# summary(fit_sev3)
# honestly I don't think it matters which to use... a model selection preference
# go with fit_sev3
performance(fit_sev3)
```

    ## # Indices of model performance
    ## 
    ## AIC     |     BIC | R2 (cond.) | R2 (marg.) |  ICC | RMSE | Sigma
    ## -----------------------------------------------------------------
    ## -496.09 | -459.57 |       0.60 |       0.30 | 0.43 | 0.05 | 25.21

``` r
icc(fit_sev3,by_group = TRUE)
```

    ## Warning: mu of 0.1 is too close to zero, estimate of random effect variances may
    ## be unreliable.

    ## # ICC by Group
    ## 
    ## Group    |   ICC
    ## ----------------
    ## MeadowId | 0.432

``` r
# this seems wrong (plus the warning message..) but maybe report just the total ICC?
#check residuals on fit_sev3
# Es.sim <- simulateResiduals(fit_sev3)
# plot(Es.sim)
# plot(Es.sim$scaledResiduals~sevTdat$sBladeArea)
# plot(Es.sim$scaledResiduals~sevTdat$sDensityShoots)
# plot(Es.sim$scaledResiduals~as.factor(sevTdat$TidalHeight))
# plot(Es.sim$scaledResiduals~sevTdat$Region)
```

## Transect level severity without CPTA n = 183

``` r
# fit_sev1 <- glmmTMB(Severity~sBladeArea+sDensityShoots+sEpiphytePerAreaMean+TidalHeight+
#                       sBladeArea:TidalHeight+sDensityShoots:TidalHeight+sEpiphytePerAreaMean:TidalHeight+
#                       (1|Region)+(1|MeadowId),
#                     data=sevT,
#                     family=beta_family(link = "logit"))
# summary(fit_sev1)
# #Region is also close to zero
# fit_sev2 <- glmmTMB(Severity~sBladeArea+sDensityShoots+sEpiphytePerAreaMean+TidalHeight+
#                       sBladeArea:TidalHeight+sDensityShoots:TidalHeight+sEpiphytePerAreaMean:TidalHeight+
#                       Region+(1|MeadowId),
#                     data=sevT,
#                     family=beta_family(link = "logit"))
# drop1(fit_sev2)
# drop region
fit_sev3 <- glmmTMB(Severity~sBladeArea+sDensityShoots+sEpiphytePerAreaMean+TidalHeight+
                      sBladeArea:TidalHeight+sDensityShoots:TidalHeight+sEpiphytePerAreaMean:TidalHeight+
                      (1|MeadowId),
                    data=sevT,
                    family=beta_family(link = "logit"))
summary(fit_sev3)
```

    ##  Family: beta  ( logit )
    ## Formula:          
    ## Severity ~ sBladeArea + sDensityShoots + sEpiphytePerAreaMean +  
    ##     TidalHeight + sBladeArea:TidalHeight + sDensityShoots:TidalHeight +  
    ##     sEpiphytePerAreaMean:TidalHeight + (1 | MeadowId)
    ## Data: sevT
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##   -580.5   -548.4    300.3   -600.5      173 
    ## 
    ## Random effects:
    ## 
    ## Conditional model:
    ##  Groups   Name        Variance Std.Dev.
    ##  MeadowId (Intercept) 0.3103   0.5571  
    ## Number of obs: 183, groups:  MeadowId, 32
    ## 
    ## Overdispersion parameter for beta family (): 26.2 
    ## 
    ## Conditional model:
    ##                                   Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)                       -2.37274    0.12795 -18.545  < 2e-16 ***
    ## sBladeArea                        -0.44262    0.09914  -4.464 8.03e-06 ***
    ## sDensityShoots                     0.45208    0.17504   2.583   0.0098 ** 
    ## sEpiphytePerAreaMean               0.25105    0.11745   2.137   0.0326 *  
    ## TidalHeightU                      -0.21191    0.12201  -1.737   0.0824 .  
    ## sBladeArea:TidalHeightU           -0.08353    0.14630  -0.571   0.5680    
    ## sDensityShoots:TidalHeightU       -0.36632    0.16013  -2.288   0.0222 *  
    ## sEpiphytePerAreaMean:TidalHeightU -0.18835    0.11691  -1.611   0.1072    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
# drop1(fit_sev3)
# that's it for model selection
# check residuals
# Es.sim <- simulateResiduals(fit_sev3)
# plot(Es.sim)
# plot(Es.sim$scaledResiduals~sevT$sBladeArea)
# plot(Es.sim$scaledResiduals~sevT$sDensityShoots)
# plot(Es.sim$scaledResiduals~as.factor(sevT$TidalHeight))
# plot(Es.sim$scaledResiduals~sevT$Region)
performance(fit_sev3)
```

    ## # Indices of model performance
    ## 
    ## AIC     |     BIC | R2 (cond.) | R2 (marg.) |  ICC | RMSE | Sigma
    ## -----------------------------------------------------------------
    ## -580.51 | -548.42 |       0.64 |       0.31 | 0.48 | 0.05 | 26.15

``` r
icc(fit_sev3, by_group = TRUE)
```

    ## Warning: mu of 0.1 is too close to zero, estimate of random effect variances may
    ## be unreliable.

    ## # ICC by Group
    ## 
    ## Group    |   ICC
    ## ----------------
    ## MeadowId | 0.480
