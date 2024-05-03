
### Summary

This project calculates Scope 3 GHG emissions for Employee Commuting. The calculation is based on Commuting Survey results for 2018, 2021, 2023, conducted by MIT Institutional Research. The emissions are broken down into 5 different commuting modes: Drive alone, Public Transportation, Carpooled, Vanpooled and Shuttle.

### Data Sources

#### Internal

* `all_scopes`: summary sheet for Scope1, 2 and 3 emission of multiple fiscal years
* `commuting_survey_2018`: Commuting Survey 2018 from IR, commuting time broken down by commuting modes and commuting modes share
* `commuting_survey_2021`: Commuting Survey 2021 from IR, total commuting time of different roles broken down by commuting modes and commuting modes share
* `commuting_survey_2023`: Commuting Survey 2023 from IR, commuting time broken down by commuting modes and commuting modes share

#### External

* `commuting_emission_factors_EPA`: Emission factors for commuting modes from EPA 2024 data.

!!! note
	U.S. Environmental Protection Agency (EPA) regularly updates a set of default emission factors for organizational greenhouse gas reporting through the EPA Emission Factors Hub. More infomation can be found [here.](https://www.epa.gov/climateleadership/ghg-emission-factors-hub)

### Processing

![commuting](../assets/images/commuting.png#shadow)

#### Derive commute distance for each mode

* Parameters and Assumptions
    * 50 work weeks
    * For 2018, public transportation combines a few sub-categories: drive/public-transportation, walk/public-transportation, bike/public-transportation, and drop-off/public-transportation. A parameter 0.8 of mixing ratio of the modes was specified.
    * Carpool emission ratio 0.4, vanpool emission ratio 0.2
    * Speed Factors and split for public transportation (consistent across survey years)
<div class="center-table" markdown>

| mode          | speed (mph) | split |
|---------------|-------------|-------|
| subway        | 20          | 0.5   |
| commuter rail | 40          | 0.35  |
| bus           | 12          | 0.14  |
| intercity     | 125         | 0.01  |
| car           | 21          |       |

</div>
* For 2018/2023, numbers in the raw table are daily total.
* For 2021, numbers in the raw table are the weekly total.
* Corresponding reply rate for each survey. `{"2018": 0.49, "2021": 0.45, "2023": 0.33}`
* For 2021, commuting time brackets weren't available. We used an average commuting time for each role (i.e. staff, faculty, graduate student etc.) based on the distribution from 2018 survey.

#### Calculate GHG emission

* Attach emission factors of corresponding commute modes.
* Scale and caculate GHG emission from commuting distance.
* Formula (output unit $MtCO_2$): 
$$ \texttt{commuting\_distance} = \texttt{commuting\_time} * \texttt{speed\_factor} * \texttt{person_count} $$
$$ \texttt{emission} = \texttt{commuting\_distance} * \texttt{emission\_factor} * \texttt{work\_week} / \texttt{reply\_rate} / 1000 $$

* Final table has GHG emissions broken down into commuting modes and survey year.
    * [2018 Survey Source](https://mit-sustainability.github.io/basin/#!/model/model.mitos.stg_commuting_survey_2018)
    * [2021 Survey Source](https://mit-sustainability.github.io/basin/#!/model/model.mitos.stg_commuting_survey_2021)
    * [2023 Survey Source](https://mit-sustainability.github.io/basin/#!/model/model.mitos.stg_commuting_survey_2023)
    * [Commuting Emission](https://mit-sustainability.github.io/basin/#!/model/model.mitos.commuting_emission#code)

#### Public available data

* We leveraged MIT data pool for project based data management and sharing.
* [Scope3 Commuting Emission](https://data.mit.edu/datahub/download/file/CD054A92D53E4A83DB5B1196C3BCA97B8658FCD5F2F1921F4F712FB9913FEDAA)

### Dashboard

* [Tableau](https://tableau.mit.edu/#/workbooks/8877?:origin=card_share_link)
* Share 4 dashboards to the MIT community
    * About: breakdown of Scopes, shared among all Scope3 dashboards
	* All Scope Overview: tree plot to show different level of drill-down of Scope 3 categories. (this is the only place used the `all_scopes` data.)
	* Commuting GHG Emission: Total emission by survey year.
	* Commuting mode: Emission broken down by commute mode, user can drill down into a specified year.
