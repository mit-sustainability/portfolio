
### Summary
This project calculates Scope 3 GHG emissions for Capital Goods associated with construction, renovation, and maintenance activities. It combines data from VPCSS (E-builder) for construction, renovation, and renewal projects, and maintenance material and service data from the facilities department. The emissions are broken down into three categories: new construction, renovations, and maintenance.



### Data Sources

#### Internal

* `all_scopes`: summary sheet for Scope1, 2 and 3 emission of multiple fiscal years.
* `construction_expense`: Raw dataset of the construction, renovation and renewal from the office of VPCSS(E-builder).
* `dof_maintenance_cost`: This dataset of maintenance material and services, from the department of facilities.

#### External

* CPI: using the CPI-U (all urban consumers) annual average values through the Python CPI library, which sources the data from U.S. Bureau of Labor Statistics. 
* `emission_factor_useeio_v2`: emission factors from USEEIOv2.0. 
* `emission_factor_naics`: Supply Chain Greenhouse Gas Emission Factors v1.2 - The datasets are comprised of greenhouse gas (GHG) emission factors (Factors) for 1,016 U.S. commodities as defined by the 2017 version of the North American Industry Classification System (NAICS).

!!! note
	Suppy Chain GHG Emission Factors v1.2 by NAICS-6 provides provides kg carbon dioxide equivalents (CO2e) per USD for all GHGs combined using 100 yr global warming potentials. The dollar (USD) in the denominator of all factors uses purchaser prices in 2021 USD. More infomation can be found [here](https://catalog.data.gov/dataset/supply-chain-greenhouse-gas-emission-factors-v1-2-by-naics-6). 

### Processing

![construction](../assets/images/construction.png#shadow)

#### Associate Supply Chain Emission Factors

* Associate USEEIO code to the Supply Chain Emission Factors v1.2
* USEEIOv2 includes 411 commidities, while Supply Chain EF v1.2 has 1,016 categories. We combined both datasets to select Supply Chain Emission Factors using USEEIO codes. 
* [Source](https://github.com/mit-sustainability/basin/blob/902fe0e9875c6924bd9b4203646489aaa0756370/orchestrator/assets/construction.py#L144-L189)


#### Attach additional info

* Adjust expense to 2021 US dollars.
* Associate emission factors based on USEEIO codes
* Calculate GHG emissions
* Construction Expense [Source](https://mit-sustainability.github.io/basin/#!/model/model.mitos.stg_construction_expense)
* Maintenance Cost [Source](https://mit-sustainability.github.io/basin/#!/model/model.mitos.stg_dof_maintenance_cost)

#### Public available data

* We leveraged MIT data pool for project based data management and sharing.
* [Processed Scope3 Construction data](https://data.mit.edu/datahub/download/file/24C174ADD45E24E1432A26AED8E695CCD6D7813729C16222E5C06D892877FCF1)

### Dashboard 

* [Tableau](https://tableau.mit.edu/views/construction_postgres/BuildingConstructionRenovationandRepair)
* Share 3 dashboards to the MIT community
	- About: breakdown of Scopes, shared among all Scope3 dashboards
	- All Scope Overview: tree plot to show different level of drill-down of Scope 3 categories. (this is the only place used the `all_scopes` data.)
	- Building Construction, Renovation and Repair: Cost and GHG emission since FY2015.
