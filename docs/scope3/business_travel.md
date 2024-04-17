## Scope3 Business Travel Overview
### Summary
This project calculates greenhouse gas (GHG) emissions from business travel using reimbursement data from VPF and emission factors from USEEIOv1.1. Its goal is to assist schools and DLCs in identifying key areas for potential climate action. This is achieved through a dashboard that displays a breakdown of expenses and emissions by expense groups, as well as trends over fiscal years.


### Data Sources

#### MIT inhouse
* `all_scopes`: summary sheet for Scope1, 2 and 3 emission of multiple fiscal years.
* `travel_spending`: Travel Spending and reimbursement data from VPF
* `Cost_Object`: using the Cost_collector_id sourced from MIT data warehouse with associated DLC\_name and School\_areas
* `Expense_category_mapper`: Mapping database for `expense type` to larger categories
* `Expense_emission_mapper`: Mapping database for `expense type` to `transport mode` required for CO2 factor matching

#### External data sources
* CPI: using the CPI-U (all urban consumers) annual average values through the Python CPI library, which sources the data from U.S. Bureau of Labor Statistics. 
* `mode_co2_mapper`: Mapping dictionary for `CO2 factor` from `transport mode`. The emission factor is sourced from USEEIOv1.1, we are using only 6 categories:

```
[
'Housing': 'Hotels and campgrounds'
'meals': ['Full-service restaurants', 'Limited-service restaurants, 'All other food and drinking places'],
'Car': 'Passenger ground Transport',
'Air': 'Air transport'
'Rail': 'Rail Transport'
'ferry': 'Water transport (boats, ships, ferries)
]
```
*notes the USEEIOv1.1 is an older model, and we will update the spend-based emission factor together with other Scope3 projects. More info can be found [here](https://www.epa.gov/land-research/us-environmentally-extended-input-output-useeio-technical-content). 

### Processing
#### Roll up of school areas
* Use the Cost\_collector\_id data from the `WAREUSER.COST_COLLECTOR` table in MIT data warehouse.
* Clean up the `cost_collector_id` and `school area` columns to remove empty values and Defunct units.
* Rename 'VP Research' to 'Interdisciplinary Research Initiatives'
* Rollup several School Areas into `MIT Administration`
* https://mit-sustainability.github.io/basin/#!/model/model.mitos.stg_cost_object_rollup#code


#### Attach Fiscal year, DLC_name, school_area, Emission Factor, CPI adjustment to the travel_spending data
* Each quarter VPF will report all travel-related reimbursement data in an excel sheet.
* The pieline will load and concatenate the new entries into the `raw.travel_spending` table in the Postgres data warehouse. We selected and only use information from 4 columns: [expense\_amount, expense\_type, trip\_end\_date, cost\_object]
* Combining all reference tables, to append DLC_name, school_area, emission_factor, CPI adjustment to the travel spending data. 
* Calculated GHG emission `mtco2`
* https://mit-sustainability.github.io/basin/#!/model/model.mitos.stg_travel_spending#code


### Dashboard
* Share 5 dashboard to the public
	- About: breakdown of Scopes, shared among all Scope3 dashboards
	- All Scope Overview: tree plot to show different level of drill-down of Scope 3 categories. (this is the only place used the `all_scopes` data.)
	- Business Travel by FY: Show travel emission/expense break down by Fiscal year and expense groups.
	- Travel Emissions vs Expenses: breakdown by expense groups
	- Business travel by school area: Emission/Expense breakdown by School area
* https://tableau.mit.edu/views/Scope3BusinessTravelpublic-Postgres/About

