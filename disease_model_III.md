Disease Models III
================
LRA
4/21/2020

## Modeling seagrass wasting disease

\*\*Update as of 2020-09-30 I updated this with most recent data
(modeled OR temps), and it gives the same result, but two major
hesitations. First, glmmTMB is the wrong package, need to refit with
betareg, which hasn’t worked so far. And 2, the mixture of remote and in
situ temps is worrying. Need to determine if this is okay or not.

This is an update to model results from Disease Models II doc, looking
at disease at the meadow scale with supplemental temperature data.
Prevalence and severity measurements on individual blades were averaged
for each meadow (3702 blades scanned across 32 meadows).

Explanatory variables were Blade Area, Shoot Density, Epiphyte Mass, and
Spring Warming.

Spring Warming was derived from MUR SST product - 1 km pixels. Because
many of our sites are in small estuaries, the coastal masking of land
excludes many sites (11 of 32 meadows). Here, I’ve added in 4 more
sites: OR-A and OR-B, in Coos Bay, where there is a NERR water quality
sampling station, and BB-F and BB-C, where I used in situ logger temps
from one of Jay’s students and temps predicted from a nearby buoy. The
NERR water temps fall between OR-A and OR-B for late summer and fall
(the period when we have HOBO loggers), so I’ve used the NERR data as
the temperature data for both OR-A and OR-B. This brings the total of
sites included in the model to 25.

The temperature variable I used was Spring Warming, I calculated the
rate of warming in ºC per week from April 1-June 30 2019.

I also included Region as a fixed effect.

Below is an abbreviated summary of the meadow level modeling with
temperature.

## Prevalence

I used beta regression to model Prevalence as the proportion of infected
blades in a meadow as a function of the parameters above.

During initial model selection, the best model had Blade Area, Spring
Warming, and Region as predictors. Spring Warming and Region were
significant predictors. Note, “sSlope” is the Spring Warming effect
(scaled slope of temp increase in Spring).

Post-hoc contrasts showed that the Regions grouped into “Super Regions”

AK and BC were in one group (not significantly different form each
other) and WA, OR, SD were in another group (not significantly different
from each other and different from the AK/BC group). These Super Regions
followed geographic patterns, so I re-grouped the meadows as Northern
(AK and BC) and Southern (WA, OR, SD). Note, BB sites were excluded for
lack of temperature data.

    ##  Family: beta  ( logit )
    ## Formula:          PrevalenceMean ~ slope + Region
    ## Data: sp
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##    -22.5    -12.5     19.3    -38.5       18 
    ## 
    ## 
    ## Overdispersion parameter for beta family (): 12.3 
    ## 
    ## Conditional model:
    ##             Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept) -7.06672    2.10640  -3.355 0.000794 ***
    ## slope       12.44657    3.53055   3.525 0.000423 ***
    ## RegionBB     4.63500    1.19464   3.880 0.000105 ***
    ## RegionBC    -0.07589    0.44546  -0.170 0.864728    
    ## RegionOR     2.79896    1.30002   2.153 0.031318 *  
    ## RegionSD     3.73156    1.25813   2.966 0.003017 ** 
    ## RegionWA     3.55695    0.88075   4.039 5.38e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    ## 
    ##   Simultaneous Tests for General Linear Hypotheses
    ## 
    ## Multiple Comparisons of Means: Tukey Contrasts
    ## 
    ## 
    ## Fit: glmmTMB(formula = PrevalenceMean ~ slope + Region, data = sp, 
    ##     family = beta_family(link = "logit"), ziformula = ~0, dispformula = ~1)
    ## 
    ## Linear Hypotheses:
    ##              Estimate Std. Error z value Pr(>|z|)    
    ## BB - AK == 0  4.63500    1.19464   3.880  0.00101 ** 
    ## BC - AK == 0 -0.07589    0.44546  -0.170  0.99996    
    ## OR - AK == 0  2.79896    1.30002   2.153  0.19757    
    ## SD - AK == 0  3.73156    1.25813   2.966  0.02522 *  
    ## WA - AK == 0  3.55695    0.88075   4.039  < 0.001 ***
    ## BC - BB == 0 -4.71089    0.96487  -4.882  < 0.001 ***
    ## OR - BB == 0 -1.83604    0.55764  -3.292  0.00903 ** 
    ## SD - BB == 0 -0.90344    0.55944  -1.615  0.49533    
    ## WA - BB == 0 -1.07805    0.58952  -1.829  0.35992    
    ## OR - BC == 0  2.87485    1.05021   2.737  0.04887 *  
    ## SD - BC == 0  3.80745    1.01266   3.760  0.00162 ** 
    ## WA - BC == 0  3.63283    0.65721   5.528  < 0.001 ***
    ## SD - OR == 0  0.93260    0.43900   2.124  0.20941    
    ## WA - OR == 0  0.75798    0.59634   1.271  0.72667    
    ## WA - SD == 0 -0.17461    0.57393  -0.304  0.99936    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## (Adjusted p values reported -- single-step method)

With the new Super Region grouping, I re-ran the model with the same
other effects (Blade Area and Spring Warming)

    ##  Family: beta  ( logit )
    ## Formula:          PrevalenceMean ~ slope + SuperRegion
    ## Data: sp
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##    -19.2    -14.2     13.6    -27.2       22 
    ## 
    ## 
    ## Overdispersion parameter for beta family (): 7.86 
    ## 
    ## Conditional model:
    ##              Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)   -7.8858     1.4395  -5.478 4.30e-08 ***
    ## slope         13.8670     2.5369   5.466 4.60e-08 ***
    ## SuperRegionS   3.9086     0.7604   5.140 2.75e-07 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

In the Super Region model, Spring Warming and Super Region were highly
significant. This suggests there is a temperature effect\!

The plots below show a model visualization - predicted values of wasting
disease prevalence at the meadow scale for each Super Region. The
relationships are quite distinct. Lines show the model predictions,
points show the empirical data.

![](disease_model_III_files/figure-gfm/prev_vis-1.png)<!-- -->

A couple notes:

If I include BB\_E, which is a bit of an outlier, there’s no significant
effect of spring warming. Overall, I’m a bit hesitant with the BB sites,
as they currently rely on temperatures predicted by modeling site temps
off of nearby buoy temps.

Also might be important to consider that here I’m combining remote
sensing and in situ measurements, which have different biases. Not
really any way around that if we want to include the 11 sites that we
can’t get remote SST.

I’m not sure what to make of the geographic split - it’s certainly
interesting but might be an artifact more than anything else.

## Severity

Finally, for severity, I used the same approach (beta regression, same
predictors)

I am still working on model parameterization here - I’m not totally
happy with the model yet. But, the significant predictors and therefore
model inference have been pretty consistent as I’m tweaking the model:
Blade Area is a significant predictor of severity at the meadow level.
Spring Warming is not.

There’s no pattern of Super Regions in the Region contrasts with
Severity. But, WA has greater severity than other sites. Perhaps because
wasting disease is so well established in WA?

    ##  Family: beta  ( logit )
    ## Formula:          SeverityMean ~ sBladeArea + sSlope + Region
    ## Data: sp
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##    -83.3    -72.0     50.7   -101.3       17 
    ## 
    ## 
    ## Overdispersion parameter for beta family (): 49.7 
    ## 
    ## Conditional model:
    ##             Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)  -2.9186     0.5172  -5.643 1.67e-08 ***
    ## sBladeArea   -1.0731     0.3117  -3.443 0.000576 ***
    ## sSlope        0.1545     0.4285   0.361 0.718427    
    ## RegionBB     -0.2880     0.9660  -0.298 0.765625    
    ## RegionBC      0.5323     0.3310   1.608 0.107733    
    ## RegionOR      1.2194     0.9744   1.251 0.210771    
    ## RegionSD     -0.1355     1.0911  -0.124 0.901139    
    ## RegionWA      1.0087     0.5854   1.723 0.084889 .  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    ## 
    ##   Simultaneous Tests for General Linear Hypotheses
    ## 
    ## Multiple Comparisons of Means: Tukey Contrasts
    ## 
    ## 
    ## Fit: glmmTMB(formula = SeverityMean ~ sBladeArea + sSlope + Region, 
    ##     data = sp, family = beta_family(link = "logit"), ziformula = ~0, 
    ##     dispformula = ~1)
    ## 
    ## Linear Hypotheses:
    ##              Estimate Std. Error z value Pr(>|z|)
    ## BB - AK == 0  -0.2880     0.9660  -0.298    0.999
    ## BC - AK == 0   0.5323     0.3310   1.608    0.497
    ## OR - AK == 0   1.2194     0.9744   1.251    0.735
    ## SD - AK == 0  -0.1355     1.0911  -0.124    1.000
    ## WA - AK == 0   1.0087     0.5854   1.723    0.423
    ## BC - BB == 0   0.8203     0.7755   1.058    0.845
    ## OR - BB == 0   1.5074     0.7529   2.002    0.265
    ## SD - BB == 0   0.1524     0.5218   0.292    0.999
    ## WA - BB == 0   1.2966     0.6243   2.077    0.230
    ## OR - BC == 0   0.6871     0.8244   0.833    0.936
    ## SD - BC == 0  -0.6679     0.8836  -0.756    0.957
    ## WA - BC == 0   0.4763     0.4266   1.117    0.815
    ## SD - OR == 0  -1.3550     0.8528  -1.589    0.510
    ## WA - OR == 0  -0.2107     0.5627  -0.375    0.998
    ## WA - SD == 0   1.1442     0.7338   1.559    0.530
    ## (Adjusted p values reported -- single-step method)
