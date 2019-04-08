Extinction Filter Database - SF Bay Area, CA
================
Amy Van Scoyoc
4/7/2019

``` r
library(tidyverse)
```

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
  arrange(desc(bodysize)) # (porcupine, sperm whale and bottlenose dolphin didn't merge)

# filter dataframe to be mammals with body size >3kg
sf_lg_mammals <- sf_traits %>%
  filter(bodysize > 2000)
length(sf_lg_mammals)
```

    ## [1] 58

### Step 2:

-   Add distribution and abundance maps for each species. (Source: *&lt;&gt;*)
