# "Ecological study measuring association between conflict, environmental factors, and annual global cutaneous and mucocutaneous leishmaniasis incidence (2005 - 2022)" data
This repository contains one dataset: ModelVars.csv. In this study, we measured the association between conflict and leishmaniasis over 18 years while accounting for other factors, such as environmental conditions and population displacement, that are relevant to leishmaniasis’ spread.



**Data Sources**

Cutaneous and mucocutaneous leishmaniasis case data (case): [WHO Global Health Observatory](https://www.who.int/data/gho/data/indicators/indicator-details/GHO/number-of-cases-of-cutaneous-leishmaniasis-reported)

Population (pop): [World Bank](https://data.worldbank.org/indicator/SP.POP.TOTL) 

Conflict intensity (con_intens): [Bertelsmann Transformation Index](https://bti-project.org/en/index/governance) 

Gross domestic product (gdp): [World Bank](https://data.worldbank.org/indicator/NY.GDP.MKTP.CD?view=chart) 

Internal displacement (displace_prop): [Internal Displacement Monitoring Center](https://www.internal-displacement.org/database/displacement-data/)

Precipitation (precip) and temperature (temp_mean, temp_range): [World Bank's Climate Change Knowledge Portal](https://climateknowledgeportal.worldbank.org/)

Humidity (hum_mean, hum_range): [NASA's Global Land Data Assimilation System Land Surface Model](https://disc.gsfc.nasa.gov/datasets?keywords=GLDAS)

Normalized difference vegetation index (ndvi): [NASA's Terra MODIS](https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MOD13A3)



**Data Specification**
- Confict intensity was lagged one year
- Displacement was included as a proportion of the total population and log-transformed
- GDP, precipitation, temperature, humidity, and NDVI were mean centered and standardized
- We included a random nation-level intercept


**Model Code**

We ran our model using the [mgcv](https://cran.r-project.org/web/packages/mgcv/mgcv.pdf) package in R version 4.0.4. The final model (Model C in Table 1) uses the following code:

`gam(case ~ year + con_intens + displace_prop_log + gdp_scale + precip_scale + s(temp_mean_scale,k=-1) + s(temp_range_scale,k=-1) + s(ndvi_scale,k=-1) + hum_mean_scale + hum_range_scale + s(nation,bs="re"), offset=log(pop), family = nb, gamma=1.5, data=ModelVars, method="REML")`
