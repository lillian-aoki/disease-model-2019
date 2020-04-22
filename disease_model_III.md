Disease Models III
================
LRA
4/21/2020

## Modeling seagrass wasting disease

This is an update to model results from Disease Models II doc, looking
at disease at the meadow scale with supplemental temperature data.
Prevalence and severity measurements on individual blades were averaged
for each meadow (3702 blades scanned across 32 meadows).

Explanatory variables were Blade Area, Shoot Density, Epiphyte Mass, and
Spring Warming.

Spring Warming was derived from MUR SST product - 1 km pixels. Because
many of our sites are in small estuaries, the coastal masking of land
excludes many sites (11 of 32 meadows). Here, I’ve added in two more
sites - OR-A and OR-B, in Coos Bay, where there is a NERR water quality
sampling station. The NERR water temps fall between OR-A and OR-B for
late summer and fall, so I’ve used the NERR data as the temperature data
for both OR-A and OR-B. This brings the total of sites included in the
model to 23.

The temperature variable I used was Spring Warming, I calculated the
rate of warming in ºC per week from April 1-June 30 2019.

I also included Region as a fixed effect.

Below is an abbreviated summary of the meadow level modeling with
temperature.

## Prevalence

I used beta regression to model Prevalence as the proportion of infected
blades in a meadow as a function of the parameters above.

During initial model selection, the best model had Blade Area, Spring
Warming, and Region as predictors. All were significant predictors.
Note, “sSlope” is the Spring Warming effect (scaled slope of temp
increase in Spring).

Post-hoc contrasts showed that the Regions grouped into “Super Regions”

AK and BC were in one group (not significantly different form each
other) and WA, OR, SD were in another group (not significantly different
from each other and different from the AK/BC group). These Super Regions
followed geographic patterns, so I re-grouped the meadows as Northern
(AK and BC) and Southern (WA, OR, SD). Note, BB sites were excluded for
lack of temperature data.

    ##  Family: beta  ( logit )
    ## Formula:          PrevalenceMean ~ sBladeArea + sSlope + Region
    ## Data: sp
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##    -21.5    -12.4     18.8    -37.5       15 
    ## 
    ## 
    ## Overdispersion parameter for beta family (): 14.5 
    ## 
    ## Conditional model:
    ##             Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)  -1.9667     0.6729  -2.923  0.00347 ** 
    ## sBladeArea   -0.2509     0.2989  -0.839  0.40120    
    ## sSlope        1.9142     0.5856   3.269  0.00108 ** 
    ## RegionBC      0.1335     0.4517   0.296  0.76750    
    ## RegionOR      3.5209     1.3847   2.543  0.01100 *  
    ## RegionSD      3.2759     1.3614   2.406  0.01612 *  
    ## RegionWA      3.6807     0.8713   4.224  2.4e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    ## 
    ##   Simultaneous Tests for General Linear Hypotheses
    ## 
    ## Multiple Comparisons of Means: Tukey Contrasts
    ## 
    ## 
    ## Fit: glmmTMB(formula = PrevalenceMean ~ sBladeArea + sSlope + Region, 
    ##     data = sp, family = beta_family(link = "logit"), ziformula = ~0, 
    ##     dispformula = ~1)
    ## 
    ## Linear Hypotheses:
    ##              Estimate Std. Error z value Pr(>|z|)    
    ## BC - AK == 0   0.1335     0.4517   0.296   0.9967    
    ## OR - AK == 0   3.5209     1.3847   2.543   0.0563 .  
    ## SD - AK == 0   3.2759     1.3614   2.406   0.0789 .  
    ## WA - AK == 0   3.6807     0.8713   4.224   <0.001 ***
    ## OR - BC == 0   3.3874     1.0957   3.092   0.0118 *  
    ## SD - BC == 0   3.1423     1.0901   2.883   0.0222 *  
    ## WA - BC == 0   3.5471     0.6082   5.833   <0.001 ***
    ## SD - OR == 0  -0.2450     0.8186  -0.299   0.9966    
    ## WA - OR == 0   0.1598     0.6591   0.242   0.9985    
    ## WA - SD == 0   0.4048     0.8057   0.502   0.9761    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## (Adjusted p values reported -- single-step method)

With the new Super Region grouping, I re-ran the model with the same
other effects (Blade Area and Spring Warming)

    ##  Family: beta  ( logit )
    ## Formula:          PrevalenceMean ~ sBladeArea + sSlope + SuperRegion
    ## Data: sp
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##    -26.9    -21.2     18.5    -36.9       18 
    ## 
    ## 
    ## Overdispersion parameter for beta family (): 14.1 
    ## 
    ## Conditional model:
    ##                     Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)          -1.9557     0.3040  -6.433 1.25e-10 ***
    ## sBladeArea           -0.1548     0.1644  -0.941    0.347    
    ## sSlope                2.0267     0.2932   6.914 4.73e-12 ***
    ## SuperRegionSouthern   3.5842     0.5261   6.812 9.60e-12 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

In the Super Region model, Spring Warming and Super Region were highly
significant. This suggests there is a temperature effect\!

The plots below show a model visualization - predicted values of wasting
disease prevalence at the meadow scale for each Super Region. The
relationships are quite distinct. Lines show the model predictions,
points show the empirical data.

![](disease_model_III_files/figure-gfm/prev_vis-1.png)<!-- -->

I am a bit hesitant about the model, partly because the BB sites aren’t
in it, and also because other temperature variables likely won’t show
the same regional split. But for now it is interesting.

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
    ##    -96.2    -87.1     56.1   -112.2       15 
    ## 
    ## 
    ## Overdispersion parameter for beta family (): 48.5 
    ## 
    ## Conditional model:
    ##             Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)  -4.1170     0.6159  -6.684 2.32e-11 ***
    ## sBladeArea   -1.0685     0.4134  -2.585  0.00975 ** 
    ## sSlope        0.6992     0.5829   1.199  0.23034    
    ## RegionBC      0.2133     0.4660   0.458  0.64725    
    ## RegionOR      2.0176     1.2896   1.564  0.11771    
    ## RegionSD      0.6479     1.4352   0.451  0.65168    
    ## RegionWA      1.9324     0.6816   2.835  0.00458 ** 
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
    ## BC - AK == 0  0.21326    0.46605   0.458  0.98408   
    ## OR - AK == 0  2.01756    1.28962   1.564  0.42483   
    ## SD - AK == 0  0.64790    1.43522   0.451  0.98485   
    ## WA - AK == 0  1.93239    0.68158   2.835  0.02667 * 
    ## OR - BC == 0  1.80430    1.06721   1.691  0.35027   
    ## SD - BC == 0  0.43465    1.14765   0.379  0.99214   
    ## WA - BC == 0  1.71913    0.49178   3.496  0.00324 **
    ## SD - OR == 0 -1.36965    1.04575  -1.310  0.59080   
    ## WA - OR == 0 -0.08517    0.76885  -0.111  0.99994   
    ## WA - SD == 0  1.28449    1.01465   1.266  0.62020   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## (Adjusted p values reported -- single-step method)
