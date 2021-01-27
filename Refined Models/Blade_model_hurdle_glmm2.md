Blade\_models\_hurdle\_glmm
================
LRA
1/26/2021

## Blade level models of prevalence and severity

Blade level models are fit as hurdles - binomial/logistic regression for
prevalence, beta regression for severity of diseased blades only

Two sets of models, one with cumulative positive temperature anomaly as
a predictor, one without.

Because only 27 meadows out of 32 have SST data for the anomaly
calculation, the number of replicates is smaller for those models.

However, the significant effects are mainly the same across the models.

Predictors are: Blade Area, Shoot Density, Epiphyte Mass per Blade Area,
Cumulative positive anomaly, Tidal Height, and all interactions between
Tidal Height and fixed effects.

Random structure for blades is Transect within Meadow within Region.
However, for the CPTA models (models that include the temperature
anomaly as a predictor), Region is not sufficiently variable to be used
as a random effect (possibly due to unbalanced availability of SST
across regions). Consistently, across all models, Region is not a
significant fixed effect either and the model can be simplified to
remove Region.

Final models therefore use Transect within Meadow for the random
structure at the blade level.

Here I am not showing all the detail of model selection and validation
(some things are commented out) so that the summary outputs are from the
final models only.

### blade level prevalence with CPTA (n=3177)

    ##  Family: binomial  ( logit )
    ## Formula:          
    ## Prevalence ~ sBladeArea + sDensityShoots + sEpiphytePerAreaMean +  
    ##     sCDiffMeanHeat + TidalHeight + sBladeArea:TidalHeight + sDensityShoots:TidalHeight +  
    ##     sEpiphytePerAreaMean:TidalHeight + sCDiffMeanHeat:TidalHeight +  
    ##     (1 | MeadowId) + (1 | TransectId)
    ## Data: dat
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##   3487.1   3559.8  -1731.5   3463.1     3165 
    ## 
    ## Random effects:
    ## 
    ## Conditional model:
    ##  Groups     Name        Variance Std.Dev.
    ##  MeadowId   (Intercept) 1.4360   1.1983  
    ##  TransectId (Intercept) 0.6854   0.8279  
    ## Number of obs: 3177, groups:  MeadowId, 27; TransectId, 162
    ## 
    ## Conditional model:
    ##                                    Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)                        0.002957   0.270198   0.011   0.9913    
    ## sBladeArea                         0.488639   0.099119   4.930 8.23e-07 ***
    ## sDensityShoots                     0.837666   0.394925   2.121   0.0339 *  
    ## sEpiphytePerAreaMean               0.314550   0.301591   1.043   0.2970    
    ## sCDiffMeanHeat                     1.142476   0.264411   4.321 1.55e-05 ***
    ## TidalHeightU                       0.297271   0.190619   1.559   0.1189    
    ## sBladeArea:TidalHeightU            0.217103   0.165071   1.315   0.1884    
    ## sDensityShoots:TidalHeightU       -0.843958   0.362902  -2.326   0.0200 *  
    ## sEpiphytePerAreaMean:TidalHeightU -0.376282   0.294575  -1.277   0.2015    
    ## sCDiffMeanHeat:TidalHeightU       -0.321523   0.178172  -1.805   0.0711 .  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    ## # Indices of model performance
    ## 
    ## AIC     |     BIC | R2 (cond.) | R2 (marg.) |  ICC | RMSE | Sigma | Log_loss | Score_log | Score_spherical
    ## ----------------------------------------------------------------------------------------------------------
    ## 3487.06 | 3559.82 |       0.49 |       0.16 | 0.39 | 0.40 |  1.00 |     0.49 |      -Inf |        3.79e-04

    ## # ICC by Group
    ## 
    ## Group      |   ICC
    ## ------------------
    ## MeadowId   | 0.265
    ## TransectId | 0.127

### Blade level prevalence, no CPTA n = 3702

This is the prevalence model without any temperature terms, with the
full 3702 blade dataset.

For this model, Region can be included in the random part

    ##  Family: binomial  ( logit )
    ## Formula:          
    ## Prevalence ~ sBladeArea + sDensityShoots + sEpiphytePerAreaMean +  
    ##     TidalHeight + sBladeArea:TidalHeight + sDensityShoots:TidalHeight +  
    ##     sEpiphytePerAreaMean:TidalHeight + (1 | Region) + (1 | MeadowId) +  
    ##     (1 | TransectId)
    ## Data: bladeWD
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##   4057.7   4126.1  -2017.9   4035.7     3691 
    ## 
    ## Random effects:
    ## 
    ## Conditional model:
    ##  Groups     Name        Variance Std.Dev.
    ##  Region     (Intercept) 1.2354   1.1115  
    ##  MeadowId   (Intercept) 1.2496   1.1178  
    ##  TransectId (Intercept) 0.6521   0.8075  
    ## Number of obs: 3702, groups:  Region, 6; MeadowId, 32; TransectId, 192
    ## 
    ## Conditional model:
    ##                                   Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)                        0.02348    0.51195   0.046   0.9634    
    ## sBladeArea                         0.41859    0.09322   4.490 7.11e-06 ***
    ## sDensityShoots                     0.81021    0.39244   2.065   0.0390 *  
    ## sEpiphytePerAreaMean               0.30397    0.24710   1.230   0.2186    
    ## TidalHeightU                       0.29277    0.17834   1.642   0.1007    
    ## sBladeArea:TidalHeightU            0.37527    0.15467   2.426   0.0153 *  
    ## sDensityShoots:TidalHeightU       -0.76616    0.35274  -2.172   0.0299 *  
    ## sEpiphytePerAreaMean:TidalHeightU -0.38350    0.23733  -1.616   0.1061    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    ## # Indices of model performance
    ## 
    ## AIC     |     BIC | R2 (cond.) | R2 (marg.) |  ICC | RMSE | Sigma | Log_loss | Score_log | Score_spherical
    ## ----------------------------------------------------------------------------------------------------------
    ## 4057.71 | 4126.09 |       0.51 |       0.04 | 0.49 | 0.40 |  1.00 |     0.49 |      -Inf |        3.19e-04

    ## # ICC by Group
    ## 
    ## Group      |   ICC
    ## ------------------
    ## Region     | 0.192
    ## MeadowId   | 0.194
    ## TransectId | 0.101

### blade level severity, with CPTA (n=1573)

Severity at the blade level is the second part of the hurdle, so it is
only modeled on the diseased blades.

Same approach to having two models, one with fewer replicates because of
the availability of SST for the anomaly calculation.

These models are fit with beta regression, as severity is a non-count
proportion.

    ##  Family: beta  ( logit )
    ## Formula:          
    ## Severity ~ sBladeArea + sDensityShoots + sEpiphytePerAreaMean +  
    ##     sCDiffMeanHeat + TidalHeight + sBladeArea:TidalHeight + sDensityShoots:TidalHeight +  
    ##     sEpiphytePerAreaMean:TidalHeight + sCDiffMeanHeat:TidalHeight +  
    ##     (1 | MeadowId) + (1 | TransectId)
    ## Data: sevdat
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##  -4755.0  -4685.3   2390.5  -4781.0     1560 
    ## 
    ## Random effects:
    ## 
    ## Conditional model:
    ##  Groups     Name        Variance Std.Dev.
    ##  MeadowId   (Intercept) 0.19346  0.4398  
    ##  TransectId (Intercept) 0.07419  0.2724  
    ## Number of obs: 1573, groups:  MeadowId, 27; TransectId, 155
    ## 
    ## Overdispersion parameter for beta family (): 7.54 
    ## 
    ## Conditional model:
    ##                                   Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)                       -2.31500    0.10417 -22.224  < 2e-16 ***
    ## sBladeArea                        -0.10551    0.05196  -2.031  0.04230 *  
    ## sDensityShoots                     0.47659    0.14861   3.207  0.00134 ** 
    ## sEpiphytePerAreaMean               0.20705    0.09710   2.132  0.03298 *  
    ## sCDiffMeanHeat                     0.14484    0.10087   1.436  0.15104    
    ## TidalHeightU                      -0.06829    0.07839  -0.871  0.38366    
    ## sBladeArea:TidalHeightU           -0.02879    0.08313  -0.346  0.72910    
    ## sDensityShoots:TidalHeightU       -0.39914    0.13702  -2.913  0.00358 ** 
    ## sEpiphytePerAreaMean:TidalHeightU -0.18211    0.09808  -1.857  0.06336 .  
    ## sCDiffMeanHeat:TidalHeightU        0.01541    0.07459   0.207  0.83634    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    ## # Indices of model performance
    ## 
    ## AIC      |      BIC | R2 (cond.) | R2 (marg.) |  ICC | RMSE | Sigma
    ## -------------------------------------------------------------------
    ## -4754.99 | -4685.30 |       0.31 |       0.06 | 0.26 | 0.12 |  7.54

    ## Warning: mu of 0.1 is too close to zero, estimate of random effect variances may
    ## be unreliable.

    ## # ICC by Group
    ## 
    ## Group      |   ICC
    ## ------------------
    ## MeadowId   | 0.186
    ## TransectId | 0.071

## blade level severity, no CPTA, n=1853

Follow up model includes all diseased blades, no temperature anomaly
term.

Also fit with beta regression.

    ##  Family: beta  ( logit )
    ## Formula:          
    ## Severity ~ sBladeArea + sDensityShoots + sEpiphytePerAreaMean +  
    ##     TidalHeight + sDensityShoots:TidalHeight + sEpiphytePerAreaMean:TidalHeight +  
    ##     (1 | Region) + (1 | MeadowId) + (1 | TransectId)
    ## Data: sev
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##  -5354.6  -5293.9   2688.3  -5376.6     1842 
    ## 
    ## Random effects:
    ## 
    ## Conditional model:
    ##  Groups     Name        Variance Std.Dev.
    ##  Region     (Intercept) 0.02135  0.1461  
    ##  MeadowId   (Intercept) 0.20158  0.4490  
    ##  TransectId (Intercept) 0.06602  0.2570  
    ## Number of obs: 1853, groups:  Region, 6; MeadowId, 32; TransectId, 183
    ## 
    ## Overdispersion parameter for beta family (): 6.84 
    ## 
    ## Conditional model:
    ##                                   Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)                       -2.20692    0.11384 -19.386  < 2e-16 ***
    ## sBladeArea                        -0.13074    0.04888  -2.675  0.00748 ** 
    ## sDensityShoots                     0.41230    0.14407   2.862  0.00421 ** 
    ## sEpiphytePerAreaMean               0.20762    0.08406   2.470  0.01351 *  
    ## TidalHeightU                      -0.09942    0.07012  -1.418  0.15626    
    ## sDensityShoots:TidalHeightU       -0.34857    0.12800  -2.723  0.00646 ** 
    ## sEpiphytePerAreaMean:TidalHeightU -0.17580    0.08336  -2.109  0.03495 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    ## # Indices of model performance
    ## 
    ## AIC      |      BIC | R2 (cond.) | R2 (marg.) |  ICC | RMSE | Sigma
    ## -------------------------------------------------------------------
    ## -5354.65 | -5293.88 |       0.31 |       0.04 | 0.28 | 0.12 |  6.84

    ## Warning: mu of 0.1 is too close to zero, estimate of random effect variances may
    ## be unreliable.

    ## # ICC by Group
    ## 
    ## Group      |   ICC
    ## ------------------
    ## Region     | 0.020
    ## MeadowId   | 0.192
    ## TransectId | 0.063
