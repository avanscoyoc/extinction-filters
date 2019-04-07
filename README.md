# Large mammal extinction filters in the San Francisco Bay Area of California

# Objective: 
Build a database with research-informed indices describing known and unknown fitness-related threats to large mammal populations in the San Francisco Bay Area of California, USA. Project this database on an interactive map for users to visualize major threats to species in the region and to toggle between individual species and different types of natural and anthropogenic threats. This tool will be made to engage local lay persons, policy makers, and researchers about known population-level threats, as well as knowledge gaps, to prioritize relative threats from local to global scales. 

# Method: 
* Create a table of native large mammals in bay area. (Source:)
* Add life history information for included species. (Source:)
* Add distribution/abundance maps for each species. (Source:)
* Add direct threats for each species. (Source: Literature)
* Add indirect threats for each species. (Source: Literature)
* Bin direct/indirect threats into categories.
* Add metric for strength and speed of threat, where possible. (Source: Literature)
* Add study types (experimental, observational, modelling) and research confidence (unknown/known, supported/contested) to each species’ threat category. (Source: Literature)
* Add individual/local/state/federal/global jurisdiction to threat categories. 
* Make a reproducible and scalable data entry method. 

# Potential resources:
* PanTHERIA
* USGS Gap Analysis Project
* IUCN Redlist
* NEON/LTER
* NatureServe
* VertNet
* DataONE

# Analysis: 
This tool will be constructed using the Leaflet package in R and the final product will be projected in RShiny. I will potentially rewrite the product to be projected with Javascript depending on speed and flexibility of the R platform. 

# Future work: 
Consider expanding this database to include all of California, the United States, or U.S. National Parks and protected areas. There are different types of threats that operate at these different scales and policy jurisdictions. Faster, spatially-explicit methods for understanding and comparing population threats would be indispensable for conservation policy, and would increase accessibility to recent research findings.  
