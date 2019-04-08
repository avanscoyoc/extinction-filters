Extinction Filter Database - SF Bay Area, CA
================
Amy Van Scoyoc
4/7/2019

    ## Warning: package 'tidyverse' was built under R version 3.4.2

    ## ── Attaching packages ───────────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.1.0     ✔ purrr   0.2.5
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.8
    ## ✔ tidyr   0.8.1     ✔ stringr 1.3.1
    ## ✔ readr   1.1.1     ✔ forcats 0.2.0

    ## Warning: package 'ggplot2' was built under R version 3.4.4

    ## Warning: package 'tibble' was built under R version 3.4.3

    ## Warning: package 'tidyr' was built under R version 3.4.4

    ## Warning: package 'purrr' was built under R version 3.4.4

    ## Warning: package 'dplyr' was built under R version 3.4.4

    ## Warning: package 'stringr' was built under R version 3.4.4

    ## ── Conflicts ──────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

### Step 1:

-   Import table of mammals in the bay area (only including native species). (Source: *<http://www.sfbaywildlife.info/species/mammals.htm>*)
-   Add life history traits to bay area mammals dataframe. (Source: *<http://esapubs.org/archive/ecol/E090/184/>* Date extracted: 4/7/2019)

``` r
# import .csv
sf_mammals <- read.csv("data/sf_mammals.csv", stringsAsFactors = FALSE) %>% 
  mutate(ID = gsub(" ", "", str_trim(scientific_name)))
```

    ## Warning: package 'bindrcpp' was built under R version 3.4.4

``` r
head(sf_mammals)
```

    ##        scientific_name          common_name                  ID
    ## 1 Didelphis virginiana    Virginia Opossum  Didelphisvirginiana
    ## 2        Sorex ornatus        Ornate Shrew         Sorexornatus
    ## 3        Sorex vagrans       Vagrant Shrew         Sorexvagrans
    ## 4        Sorex sonomae           Fog Shrew         Sorexsonomae
    ## 5    Sorex trowbridgii  Trowbridge's Shrew     Sorextrowbridgii
    ## 6 Neurotrichus gibbsii American Shrew mole  Neurotrichusgibbsii

``` r
# import life history traits from PanTHERIA
pantheria <- read.table("http://esapubs.org/archive/ecol/E090/184/PanTHERIA_1-0_WR05_Aug2008.txt", sep = "\t", stringsAsFactors = FALSE, header = TRUE, na.strings = c("NA", "-999", "-999.00")) %>% 
  mutate(ID = gsub(" ", "", str_trim(MSW05_Binomial)))
names(pantheria)[7] <- "bodysize"

# join life history traits to Bay Area mammal dataframe
sf_traits <- left_join(sf_mammals, pantheria, by="ID") %>%  
  arrange(desc(bodysize))
head(sf_traits)  # (porcupine, sperm whale and bottlenose dolphin didn't merge)
```

    ##          scientific_name           common_name                    ID
    ## 1  Balaenoptera musculus           Blue Whale   Balaenopteramusculus
    ## 2  Balaenoptera physalus            Fin Whale   Balaenopteraphysalus
    ## 3 Megaptera novaeangliae       Humpback Whale  Megapteranovaeangliae
    ## 4  Eschrichtius robustus           Gray Whale   Eschrichtiusrobustus
    ## 5  Balaenoptera borealis            Sei Whale   Balaenopteraborealis
    ## 6      Berardius bairdii Baird's Beaked Whale       Berardiusbairdii
    ##   MSW05_Order    MSW05_Family  MSW05_Genus MSW05_Species
    ## 1     Cetacea Balaenopteridae Balaenoptera      musculus
    ## 2     Cetacea Balaenopteridae Balaenoptera      physalus
    ## 3     Cetacea Balaenopteridae    Megaptera  novaeangliae
    ## 4     Cetacea  Eschrichtiidae Eschrichtius      robustus
    ## 5     Cetacea Balaenopteridae Balaenoptera      borealis
    ## 6     Cetacea       Ziphiidae    Berardius       bairdii
    ##           MSW05_Binomial X1.1_ActivityCycle  bodysize
    ## 1  Balaenoptera musculus                 NA 154321304
    ## 2  Balaenoptera physalus                 NA  47506008
    ## 3 Megaptera novaeangliae                 NA  30000000
    ## 4  Eschrichtius robustus                  2  27324024
    ## 5  Balaenoptera borealis                 NA  22106252
    ## 6      Berardius bairdii                 NA  11380000
    ##   X8.1_AdultForearmLen_mm X13.1_AdultHeadBodyLen_mm X2.1_AgeatEyeOpening_d
    ## 1                      NA                  30480.00                     NA
    ## 2                      NA                  20641.06                     NA
    ## 3                      NA                  12856.21                     NA
    ## 4                      NA                  11831.89                     NA
    ## 5                      NA                  18382.16                     NA
    ## 6                      NA                        NA                     NA
    ##   X3.1_AgeatFirstBirth_d X18.1_BasalMetRate_mLO2hr X5.2_BasalMetRateMass_g
    ## 1                     NA                        NA                      NA
    ## 2                     NA                        NA                      NA
    ## 3                     NA                        NA                      NA
    ## 4                     NA                        NA                      NA
    ## 5                     NA                        NA                      NA
    ## 6                     NA                        NA                      NA
    ##   X6.1_DietBreadth X7.1_DispersalAge_d X9.1_GestationLen_d
    ## 1                1                  NA              326.97
    ## 2                2                  NA              338.36
    ## 3                2                  NA              349.79
    ## 4                3                  NA              379.90
    ## 5                2                  NA              334.58
    ## 6                2                  NA              396.58
    ##   X12.1_HabitatBreadth X22.1_HomeRange_km2 X22.2_HomeRange_Indiv_km2
    ## 1                    1                  NA                        NA
    ## 2                    1                  NA                        NA
    ## 3                    1                  NA                        NA
    ## 4                    1                  NA                        NA
    ## 5                    1                  NA                        NA
    ## 6                    1                  NA                        NA
    ##   X14.1_InterbirthInterval_d X15.1_LitterSize X16.1_LittersPerYear
    ## 1                     821.25             1.00                 0.45
    ## 2                     730.00             1.01                 0.37
    ## 3                     730.00             1.00                 0.37
    ## 4                     638.75             1.00                 0.50
    ## 5                     730.00             1.02                 0.41
    ## 6                         NA             0.98                 0.33
    ##   X17.1_MaxLongevity_m X5.3_NeonateBodyMass_g X13.2_NeonateHeadBodyLen_mm
    ## 1                 1320                2738613                     7236.55
    ## 2                 1392                1900000                     6273.75
    ## 3                 1140                1300000                          NA
    ## 4                  924                 500000                     4430.00
    ## 5                  888                 650000                     4729.99
    ## 6                  852                     NA                          NA
    ##   X21.1_PopulationDensity_n.km2 X10.1_PopulationGrpSize
    ## 1                            NA                     1.0
    ## 2                            NA                     1.5
    ## 3                            NA                      NA
    ## 4                           4.9                    16.0
    ## 5                            NA                      NA
    ## 6                            NA                    11.5
    ##   X23.1_SexualMaturityAge_d X10.2_SocialGrpSize X24.1_TeatNumber
    ## 1                   1959.80                1.25               NA
    ## 2                   2666.41                  NA               NA
    ## 3                   1310.35                2.00               NA
    ## 4                   3189.25               16.00               NA
    ## 5                   3274.59                4.25               NA
    ## 6                   5050.97               30.00               NA
    ##   X12.2_Terrestriality X6.2_TrophicLevel X25.1_WeaningAge_d
    ## 1                   NA                 3             211.71
    ## 2                   NA                 3             196.58
    ## 3                   NA                 3             318.46
    ## 4                   NA                 2             211.71
    ## 5                   NA                 3             203.46
    ## 6                   NA                 3                 NA
    ##   X5.4_WeaningBodyMass_g X13.3_WeaningHeadBodyLen_mm
    ## 1                1.7e+07                          NA
    ## 2                     NA                       12000
    ## 3                     NA                          NA
    ## 4                     NA                          NA
    ## 5                     NA                          NA
    ## 6                     NA                          NA
    ##                                                           References
    ## 1                 172;511;543;899;1004;1015;1217;1297;2151;2409;2655
    ## 2                   24;27;543;899;1004;1015;1217;1297;1577;2151;2655
    ## 3 24;68;159;162;163;179;490;491;505;543;899;1004;1297;1577;2151;2655
    ## 4                           388;505;511;543;1004;1015;1297;1513;2655
    ## 5                               511;543;899;1004;1217;1297;2151;2655
    ## 6                                        511;543;1015;1297;2151;2655
    ##   X5.5_AdultBodyMass_g_EXT X16.2_LittersPerYear_EXT
    ## 1                       NA                       NA
    ## 2                       NA                       NA
    ## 3                       NA                       NA
    ## 4                       NA                       NA
    ## 5                       NA                       NA
    ## 6                       NA                       NA
    ##   X5.6_NeonateBodyMass_g_EXT X5.7_WeaningBodyMass_g_EXT X26.1_GR_Area_km2
    ## 1                         NA                         NA                NA
    ## 2                         NA                    6395530                NA
    ## 3                         NA                         NA                NA
    ## 4                         NA                         NA                NA
    ## 5                         NA                         NA                NA
    ## 6                         NA                         NA                NA
    ##   X26.2_GR_MaxLat_dd X26.3_GR_MinLat_dd X26.4_GR_MidRangeLat_dd
    ## 1                 NA                 NA                      NA
    ## 2                 NA                 NA                      NA
    ## 3                 NA                 NA                      NA
    ## 4                 NA                 NA                      NA
    ## 5                 NA                 NA                      NA
    ## 6                 NA                 NA                      NA
    ##   X26.5_GR_MaxLong_dd X26.6_GR_MinLong_dd X26.7_GR_MidRangeLong_dd
    ## 1                  NA                  NA                       NA
    ## 2                  NA                  NA                       NA
    ## 3                  NA                  NA                       NA
    ## 4                  NA                  NA                       NA
    ## 5                  NA                  NA                       NA
    ## 6                  NA                  NA                       NA
    ##   X27.1_HuPopDen_Min_n.km2 X27.2_HuPopDen_Mean_n.km2
    ## 1                       NA                        NA
    ## 2                       NA                        NA
    ## 3                       NA                        NA
    ## 4                       NA                        NA
    ## 5                       NA                        NA
    ## 6                       NA                        NA
    ##   X27.3_HuPopDen_5p_n.km2 X27.4_HuPopDen_Change X28.1_Precip_Mean_mm
    ## 1                      NA                    NA                   NA
    ## 2                      NA                    NA                   NA
    ## 3                      NA                    NA                   NA
    ## 4                      NA                    NA                   NA
    ## 5                      NA                    NA                   NA
    ## 6                      NA                    NA                   NA
    ##   X28.2_Temp_Mean_01degC X30.1_AET_Mean_mm X30.2_PET_Mean_mm
    ## 1                     NA                NA                NA
    ## 2                     NA                NA                NA
    ## 3                     NA                NA                NA
    ## 4                     NA                NA                NA
    ## 5                     NA                NA                NA
    ## 6                     NA                NA                NA

``` r
# filter dataframe to be mammals with body size >3kg
sf_lg_mammals <- sf_traits %>%
  filter(bodysize > 2000)
length(sf_lg_mammals)
```

    ## [1] 58

### Step 2:

-   Add distribution and abundance maps for each species. (Source: *&lt;&gt;*)
