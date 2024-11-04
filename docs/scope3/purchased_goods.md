
### Summary

This project calculates greenhouse gas (GHG) emissions from purchased goods and services (PG&S) at MIT using a spend-based approach and USEEIOv2.0 emission factors. It supports MIT's Climate Action Plan, aiming to identify reduction opportunities by classifying commodity categories as actionable or non-actionable. The project provides a dashboard for transparent reporting and trend analysis over fiscal years.

### Data Sources

#### Internal

* [Buy2Pay Portal (login required)](https://mit.coupahost.com/user/home): Tracks all MIT purchases, allowing categorization by supplier, commodity type, and cost. Organized into ~320 commodity category, with duplicative categories removed to avoid double-counting.
* `Cost_Object`: using the Cost_collector_id sourced from MIT data warehouse with associated DLC\_name and School\_areas
* `all_scopes`: summary sheet for Scope1, 2 and 3 emission of multiple fiscal years.
* `purchased_goods_mapping`: mapping from commodity categories to commidity with USEEIO codes and emission factors.


#### External

* USEEIOv2 Emission Factors: Used to estimate emissions by matching MIT's commodity categories to NAICS codes, shared among multiple pipelines.
* Consumer Price Index (CPI): Adjusts expenses to a reference year (2021) for consistency.

!!! note 
	The USEEIOv2.0 model, developed by the U.S. EPA, uses economic-environmental input-output data from 2012 and includes emission factors across multiple industries. More information is available [here](https://www.epa.gov/land-research/us-environmentally-extended-input-output-useeio-technical-content).

### Processing

![Puchased goods and services](pns.png#shadow)

* Download annually invoice lines from Buy2Pay.
* Adjust spending total for inflation using CPI.
* Assign USEEIO codes and emission factors to commodity categories
* Multiplies adjusted spending by emission factors for each category to estimate GHG emissions.
* Create summary tables grouping by: purchased_order_number, supplier_number, and level_3 commodity category.

### Dashboard

#### [Tableau dashboard](https://tableau.mit.edu/#/views/FY2024Scope3PurchasedGoodsandServicesOct29/Actionable_Select?)

* Create sets: ALL, ACTIONABLE (180 commodity types with potential for reduction), NOT ACTIONABLE, or DUPLICATED.
* All Categories: Visualizes emissions for all commodity categories.
* Spending Trends: Displays yearly breakdowns for transparency and tracking progress toward emission reduction goals (to be implemented.)
