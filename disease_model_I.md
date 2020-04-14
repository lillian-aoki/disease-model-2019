Disease Models 1
================
LRA
4/13/2020

## Modeling seagrass wasting disease

Here, I modeled seagrass wasting disease using a GLMM hurdle model
approach.

Underlying data were from the 2019 NSF surveys. 3702 individual blades
were scanned for disease prevalence and severity.

Explanatory factors were Tidal Height, Shoot Density, and Blade Area.
From earlier work in the SJI, we would expect that disease prevalence
and severity would increase with larger blade areas and greater shoot
densities.

Random effects were MeadowId (site within region). I also looked at
effect of grouping by region.

\*Note, this markdown file doesn’t include all details of the model
selection and validation process, for simplicity in sharing results

    ##  Family: binomial  ( logit )
    ## Formula:          
    ## Prevalence ~ sBladeArea + sDensityShoots + TidalHeight + (1 |  
    ##     MeadowId)
    ## Data: bladeWD
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##   4199.3   4230.4  -2094.6   4189.3     3697 
    ## 
    ## Random effects:
    ## 
    ## Conditional model:
    ##  Groups   Name        Variance Std.Dev.
    ##  MeadowId (Intercept) 1.965    1.402   
    ## Number of obs: 3702, groups:  MeadowId, 32
    ## 
    ## Conditional model:
    ##                Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)     0.15978    0.25532   0.626 0.531443    
    ## sBladeArea      0.22316    0.06630   3.366 0.000763 ***
    ## sDensityShoots -0.04764    0.05912  -0.806 0.420370    
    ## TidalHeightU    0.13448    0.08730   1.540 0.123444    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

The first part of the model shows that Blade Area is a significant
predictor of disease status. Tidal Height and Shoot Density were not
significant predictors.

Note, the predictors are centered and scaled here (for each value, the
mean is subtracted and then the difference is divided by one standard
deviation). Scaling prevents numerical issues with model convergence
with predictors of very different units. Therefore, to interpret the
model output, a coefficient of 0.5 for a predictor means an increase of
0.5 in the response variable for every unit of SD in the predictor. E.g.
here for Blade Area, the coefficient is 0.22; if Blade Area increases by
1 SD, the probability of disease increases by 0.22.

Second note, the model is fitted with MeadowId as the random (grouping)
effect. Differences between meadows were greater than differences
between regions.

    ##  Family: beta  ( logit )
    ## Formula:          
    ## Severity ~ sBladeArea + sDensityShoots + TidalHeight + (1 | MeadowId)
    ## Dispersion:                ~Region
    ## Data: diseased
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##  -6208.8  -6147.5   3115.4  -6230.8     1944 
    ## 
    ## Random effects:
    ## 
    ## Conditional model:
    ##  Groups   Name        Variance Std.Dev.
    ##  MeadowId (Intercept) 0.4506   0.6712  
    ## Number of obs: 1955, groups:  MeadowId, 32
    ## 
    ## Conditional model:
    ##                Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)    -2.43726    0.12669 -19.238   <2e-16 ***
    ## sBladeArea     -0.10861    0.04297  -2.527   0.0115 *  
    ## sDensityShoots  0.04999    0.04439   1.126   0.2601    
    ## TidalHeightU    0.05865    0.04988   1.176   0.2397    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Dispersion model:
    ##             Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)   1.5946     0.0727  21.933  < 2e-16 ***
    ## RegionBB      0.7807     0.1041   7.500 6.37e-14 ***
    ## RegionBC     -0.4122     0.1284  -3.210  0.00133 ** 
    ## RegionOR      1.0952     0.1921   5.700 1.20e-08 ***
    ## RegionSD     -0.3572     0.1165  -3.066  0.00217 ** 
    ## RegionWA      0.4363     0.1048   4.163 3.14e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Second part of the model shows that Blade Area is signiciant but p-value
is fairly large - esp for GLMM. Tidal Height and Shoot Density were
agian not significant.

![](disease_model_I_files/figure-gfm/sev-1.png)<!-- -->

## Model predictions

These are predictions looking at how probability of disease and disease
severity behave with increasing blade area.

In these plots, Shoot Density is held constant at the mean value (311
shoots per m2) and Tidal Height is held at Lower. Since those predictors
were not significant in the models, the constant values shouldn’t
matter.

![](disease_model_I_files/figure-gfm/prev_plot-1.png)<!-- -->

The black line shows the means of simulated model output, and the dashed
lines show 95% confidence intervals. From the plot, we can see that our
smallest blades (1 cm2), have about a 50% chance of being infected,
whereas the largest blades (250 cm2) have about a 75% chance of being
infected. But the 95% CI is huge. Blade Area isn’t a great predictor of
disease at the blade level.

![](disease_model_I_files/figure-gfm/sev_plot-1.png)<!-- -->

Again the black line indicates the model prediction and the dashed lines
show the model 95% CI. Grey points are the underlying data. There’s a
slight decline in severity moving from smaller to larger blades, which
is the opposite trend as for Prevalence. Although, it’s not totally
unreasonable to think that larger blades are more likely to be infected
but have less sever infections, just because of the larger blade area.

Again, the data are very noisy due to high level of spread for
individual blades.

At this point, my plan is to move on to modeling at the transect level,
including some temperature data from loggers and satellite imagery.
