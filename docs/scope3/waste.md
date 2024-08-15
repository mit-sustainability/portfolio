
### Summary

This project calculates Scope 3 GHG emissions for waste generated in MIT operations and third-party disposal and treatment. Data on MIT-generated waste includes tonnage provided by waste haulers (see [Material Matters](https://datapool.mit.edu/visualization/materials-matters-mits-waste-and-recycling-data)) and small stream recycling items sourced from the facilities.


### Data Sources

#### Internal

* `all_scopes`: summary sheet for Scope 1, 2 and 3 emission of multiple fiscal years.
* `small_stream_recycle`: This sheet is updated and provided by our partners at Facilities, logging the weight and amount of hard-to-recycle items including clothing, tires, mattresses, books, e-waste, and metal.

#### External

* `newbatch_waste_recycle`: the raw dataset of the waste tonnage provided by the waste hauler, broken down by material stream, including Trash, Food Waste, Recycling, C&D, Yard Waste, and Other.
* `waste_emission_factors_epa`: Emission factors for waste and recycled materials from EPA Emission Factor Hub (June 2024).

!!! note
	Our methodology followed the GHG protocol scope 3 category 5 to calculate GHG emissions associated with generated waste, not accounting for the "avoided emissions" due to recycling or waste processing improvements downstreams.

### Processing

![Scope 3 waste data lineage](../assets/images/waste.png#shadow)

The scope3 waste GHG emission calculation extends the [Material Matters](https://datapool.mit.edu/visualization/materials-matters-mits-waste-and-recycling-data) pipeline by associating emission factors of various material streams with specified adjustments and splits.

#### Associate Emission Factors by Material Streams

* Associate emission factors by material streams
* Category mapping:

<div class="center-table" markdown>
| Material Matters           | Mapped Category   | Recycled | Landfilled | Combusted | Composted | Equivalent Emission Factor |
|--------------------------| -----------------|--------|----------| ---------|---------|--------------------------|
| C & D<sup>†</sup> | Mixed MSW         | N/A\*    | 0.58\*\*   | 0.43      | N/A       | Landfilled                 |
| Compost                    | Food Waste        | N/A      | 0.68       | 0.05      | 0.11      | Composted                  |
| Hard-to-recycle Materials<sup>†</sup> | Mixed MSW         | N/A      | 0.58       | 0.43      | N/A       | (Landfilled + combusted)/2 |
| Other                      | Mixed Organics    | N/A      | 0.54       | 0.05      | 0.13      | (Landfilled + combusted)/2 |
| Recycling<sup>†</sup> | Mixed Recyclables | 0.09     | 0.75       | 0.11      | N/A       | Recycled                   |
| Trash                      | Mixed MSW         | N/A      | 0.54       | 0.05      | 0.13      | (Landfilled + combusted)/2 |
| Yard Waste                 | Yard Trimmings    | N/A      | 0.36       | 0.05      | 0.14      | Composted                  |
</div>

???+ Info
	- <sup>†</sup>90% of Hard-to-recycle and 50% of C&D go to Recycling, 22% of Recycling goes to Trash
	- \*Any emission factor with an ‘N/A’ value was assumed to be zero in calculations
	- \*\*Emission factor has the unit metric ton CO2 per short ton of waste

* Weight of material streams such as C&D, Trash, Recycling, and Hard-to-recycle Materials were adjusted based on our past experience and [research](https://sustainability.mit.edu/sites/default/files/resources/2020-12/dissertation_rachel_perlman_2020.pdf).
* [Source](https://mit-sustainability.github.io/basin/#!/model/model.mitos.stg_waste_emission#code)


#### Calculate the GHG emission

* The weight of each material stream is first aggregated into a monthly total, timed by the corresponding emission factors, and converted to GHG emissions in metric tons.

#### Public available data

* Processed scope 3 annual waste emission broken down by the material stream can be downloaded from MIT's Data Hub.
* [Scope 3 waste emission by fiscal year](https://data.mit.edu/datahub/download/file/7352EAB740D1849426BB9861779FA61D10F524A408A022F065C7BA2EF5D204AA)

### Dashboard 

* [Tableau](https://tableau.mit.edu/views/Scope3WasteEmission/waste_emission/4b255dda-703d-45e3-b1d2-d548a6c190a4/7f466410-17cf-477e-9250-ed3b33a68958)
* Share three dashboards to the MIT community.
	- All Scope Overview: breakdown of Scopes, shared among all Scope3 dashboards
	- Waste Emission: annual total CO<sub>2</sub> emission associated with generated waste
	- CO<sub>2</sub> Weight Share: annual CO<sub>2</sub> and waste tonnage share broken down by material streams
