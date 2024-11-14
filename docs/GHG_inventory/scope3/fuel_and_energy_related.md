
### Summary

This pipeline calculates the indirect GHG emissions of fuel and energy-related activity based on purchased fuel and electricity sourced from `energy-cdr`.


### Data Sources

#### Internal

* **`purchased_energy`**: Loaded from `energize-mit`, filtered to distinguish `CUP` and `Non-CUP` purchased energy, aligning with Scope 1 and 2 emissions shown in `energize-mit`.

#### External

* `upstream electricity factor`: 0.0000668 [MtCO2e/kWH], life cycle emission factors corresponding to national electricity grids released by [IEA](https://www.iea.org/data-and-statistics/data-product/life-cycle-upstream-emission-factors-pilot-edition).
* `upstream gas factor`: 0.0003366 [MtCO2e/cubic_meters] from the UK Department for Environment Food & Rural Affairs [Conversion Factors 2023](https://www.gov.uk/government/publications/greenhouse-gas-reporting-conversion-factors-2023) spreadsheet (on the "WTT-fuels" worksheet).
* `upstream fuel factor`: 0.00069539 [MtCO2e/litter] from the UK Defra [Conversion Factors 2023](https://www.gov.uk/government/publications/greenhouse-gas-reporting-conversion-factors-2023) spreadsheet (on the "WTT-fuels" worksheet).
* `T&D lost factor`: 5.1% for Eastern grid from EPA [eGRID 2022](https://www.epa.gov/egrid/download-data) data.

!!! note
	We closely followed the practice suggested by [EPA](https://www.epa.gov/climateleadership/scope-3-inventory-guidance#factors) and the [GHG protocol](https://ghgprotocol.org/sites/default/files/2022-12/Chapter3.pdf). The term has three components relevant to MIT's operation:

    	A. Upstream emissions of purchased fuels
        B. Upstream emissions of purchased electricity
        C. Transmission and distribution (T&D) losses

### Processing

1. Ingest `purchased_energy` data from `energy-cdr`.
2. Calculate T&D loss using total purchased electricity (CUP and Non_CUP).
3. Convert the gas unit from CCF to cubic meters and multiply by the factor.
4. Convert the fuel oil unit from gallons to liters and multiply by the factor.
5. Calculate upstream electricity emission using the total purchased electricity.
6. Aggregate the numbers above.
