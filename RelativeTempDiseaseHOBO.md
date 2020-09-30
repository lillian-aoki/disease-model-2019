RelativeTempDiseaseHOBO
================
LRA
9/30/2020

## Spatial temperature anomaly and disease

Here I am using the in situ temperatures to determine if sites that are
hotter locally, i.e. compared to other sites within the same region,
have higher rates of disease.

Using all the HOBO data from JJA of 2019. Combined upper and lower tidal
heights, as there’s little consistent variation between
them.

![](RelativeTempDiseaseHOBO_files/figure-gfm/plots-1.png)<!-- -->![](RelativeTempDiseaseHOBO_files/figure-gfm/plots-2.png)<!-- -->

#### July and August

Focus on July and August (temp records for June are incomplete)

Potential metrics of spatial temperature anomaly: 1) Mean daily
difference between site temps and the regional mean 2) Proportion of
days in July and Aug spent with daily temp above the regional mean

Note, use the proportion of days out of days when in situ loggers were
deployed so that sites with longer records aren’t biased.

Can calculate the same as above for 90th percentile instead of regional
mean

![](RelativeTempDiseaseHOBO_files/figure-gfm/data2-1.png)<!-- -->![](RelativeTempDiseaseHOBO_files/figure-gfm/data2-2.png)<!-- -->![](RelativeTempDiseaseHOBO_files/figure-gfm/data2-3.png)<!-- -->![](RelativeTempDiseaseHOBO_files/figure-gfm/data2-4.png)<!-- -->![](RelativeTempDiseaseHOBO_files/figure-gfm/data2-5.png)<!-- -->![](RelativeTempDiseaseHOBO_files/figure-gfm/data2-6.png)<!-- -->![](RelativeTempDiseaseHOBO_files/figure-gfm/data2-7.png)<!-- -->![](RelativeTempDiseaseHOBO_files/figure-gfm/data2-8.png)<!-- -->

No overall pattern with spatial temperature anomaly metrics and disease.
In WA and AK, sites that are warm for the region do show an increase in
prevalence, but for other regions, the pattern disappears.

So, a spatial anomaly may be important in some regions but no uniform
effect.

Removing BB\_E (exceptionally warm for its region) doesn’t change
patterns.

Plots are not shown for 90th percentile metrics, but the lack of pattern
is the same as for the metrics shown.

#### JJA

These are the same plots but using summer temperatures from the period
June-Aug. More uneven betwen regions, but still don’t see any
patterns.

![](RelativeTempDiseaseHOBO_files/figure-gfm/jja-1.png)<!-- -->![](RelativeTempDiseaseHOBO_files/figure-gfm/jja-2.png)<!-- -->![](RelativeTempDiseaseHOBO_files/figure-gfm/jja-3.png)<!-- -->![](RelativeTempDiseaseHOBO_files/figure-gfm/jja-4.png)<!-- -->![](RelativeTempDiseaseHOBO_files/figure-gfm/jja-5.png)<!-- -->![](RelativeTempDiseaseHOBO_files/figure-gfm/jja-6.png)<!-- -->![](RelativeTempDiseaseHOBO_files/figure-gfm/jja-7.png)<!-- -->![](RelativeTempDiseaseHOBO_files/figure-gfm/jja-8.png)<!-- -->

Overall, we see no indication that spatial temperature anomalies can
explain disease across regions.

I don’t plan to present any models to explain these non-patterns.

Overall
