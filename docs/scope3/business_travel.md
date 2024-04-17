
### Summary
This project calculates greenhouse gas (GHG) emissions from business travel using VPF reimbursement data and USEEIOv1.2 emission factors. It aims to help schools and DLCs identify areas for climate action by providing a dashboard that visualizes expense breakdowns, emissions by expense group, and changes over fiscal years.


### Data Sources

#### Internal
* `all_scopes`: summary sheet for Scope1, 2 and 3 emission of multiple fiscal years.
* `travel_spending`: Travel Spending and reimbursement data from VPF
* `Cost_Object`: using the Cost_collector_id sourced from MIT data warehouse with associated DLC\_name and School\_areas
* `Expense_emission_mapper`: Mapping database for `expense type` to `transport mode` required for CO2 factor matching

#### External
* CPI: using the CPI-U (all urban consumers) annual average values through the Python CPI library, which sources the data from U.S. Bureau of Labor Statistics. 
* `emission_factor_useeio_v2`: emission factors from USEEIOv1.2, based on 2012 US dollar. 

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

!!! note
	The USEEIOv2.0 is a combined economic-environmental model. The emission factor table is shared across multiple Scope3 projects. More info can be found [here](https://www.epa.gov/land-research/us-environmentally-extended-input-output-useeio-technical-content). 

### Processing
![business_travel](../assets/images/business_travel_flow.png#shadow)

#### Roll up of school areas
* Use the Cost\_collector\_id data from the `WAREUSER.COST_COLLECTOR` table in MIT data warehouse.
* Clean up the `cost_collector_id` and `school area` columns to remove empty values and Defunct units.
* Rename 'VP Research' to 'Interdisciplinary Research Initiatives'
* Rollup several School Areas into `MIT Administration`
* https://mit-sustainability.github.io/basin/#!/model/model.mitos.stg_cost_object_rollup#code


#### Attach additional info
* Each quarter VPF will report all travel-related reimbursement data in an excel sheet.
* The pieline will load and concatenate the new entries into the `raw.travel_spending` table in the Postgres data warehouse. We selected and only use information from 4 columns: [expense\_amount, expense\_type, trip\_end\_date, cost\_object]
* Combining all reference tables, to append DLC\_name, school\_area, emission\_factor, CPI adjustment to the travel spending data.
* Calculated GHG emission `mtco2`
* https://mit-sustainability.github.io/basin/#!/model/model.mitos.stg_travel_spending#code

#### Public available data
* We leveraged MIT data pool for project based data management and sharing.
* [Processed Travel Spending Data](https://data.mit.edu/datahub/download/file/5BEBE86F9BDEDE3AC0507356CAD717D966DCC4CEB4F91B716CFF8D527EDC65B2)

### Dashboard 
* [Tableau](https://tableau.mit.edu/views/Scope3BusinessTravelpublic-Postgres/About)
* Share 5 dashboards to the MIT community
	- About: breakdown of Scopes, shared among all Scope3 dashboards
	- All Scope Overview: tree plot to show different level of drill-down of Scope 3 categories. (this is the only place used the `all_scopes` data.)
	- Business Travel by FY: Show travel emission/expense break down by Fiscal year and expense groups.
	- Travel Emissions vs Expenses: breakdown by expense groups
	- Business travel by school area: Emission/Expense breakdown by School area
