<img src="tutorials\PVDAQ_logo.png" width="100">

# U.S. Agrivoltaic Irradiance Database

## Overview
This is a foundational data set for research and deployment of agrivoltaics, which is the co-location of agriculture and solar power plants on the same land. This irradiance and shading dataset can be utilized to determine the suitability of agrivoltaics configurations for a given region and crop-type. The data is hourly, 4x4 km resolution across the contiguous United States and Hawaii. It is calculated from National Solar Radiation Database (NSRDB) sites, using the System Advisor Model (SAM) to simulate the shading patterns for 11 common agrivoltaics configurations. Sunlight availability data for a typical meteorological year (TMY) is reported at 10 locations on the ground between adjacent rows of solar panels, as well as averaged across areas of interest such as the average irradiance in the edge-to-edge open area or across 3-6 planting beds. Other available metrics include the input meteorological data from the NSRDB (e.g. global horizontal irradiance, wind speed, etc.) and estimates for comparing energy and agricultural characteristic across the 11 configurations, including power output per acre or per kW installed capacity and farmable land area per acre.

Data is available through the [OEDI Data Lake](https://data.openei.org/submissions/4568)(Ref [1]). You can also access this data via the InSPIRE Shading Tool at https://openei.org/wiki/InSPIRE/Agrivoltaics_Shading_Tool. This software module allows a user to downlaod data to their local system from the OEDI Data Lake.


## Geographic/temporal scope and resolution

### Spatial
* 4x4 km resolution across the contiguous United States and Hawaii (Alaska is excluded)
* This data uses NSRDB's system of spatial indeces or grid ID's (GID) for each Coordinate position. The gid-lat-lon.csv file supports the conversion of coordinates to GIDs for easy access of the data. 

### Temporal
* Hourly data over a single year. 

## Key tools and datasets used
This dataset was generated using the National Solar Radiation Database (NSRDB)[https://github.com/openEDI/documentation/blob/main/NSRDB.md] for the which is a serially complete collection of hourly and half-hourly values of meteorological data and the three most common measurements of solar radiation and System Advisor Model (SAM) to simulate the shading patterns for 11 common agrivoltaics configurations.

# File structure and information

The data is made available as a series of .zarr files corresponding to each configuration modeled and can be found at s3://nrel-pds-nsrdb/v3/nsrdb_${year}.h5

The variables mentioned above are provided in 2 dimensional time-series arrays with dimensions (time x location). The temporal axis is defined by the time_index dataset, while the positional axis is defined by the meta dataset. The units for the variable data is also provided as an attribute (psm_units).

## File structure
```
├──v1.0  #
│  ├──configuration_01.zarr/		
│  ├──configuration_02.zarr/		
│  ├──configuration_03.zarr/		
│  ├──configuration_04.zarr/		
│  ├──configuration_05.zarr/		
│  ├──configuration_06.zarr/		
│  ├──configuration_07.zarr/		
│  ├──configuration_08.zarr/		
│  ├──configuration_09.zarr/		
│  ├──configuration_10.zarr/
│  ├──configuration_11.zarr/
│  ├──gid-lat-lon.csv
```

## Definitions and variables

### Configurations

| Number | Description |
|:-------|:------------|
| 1      | Conventional single-axis tracking |
| 2      | Single-axis tracking with elevated panels |
| 3      | Single-axis tracking with elevated and inter-row spaced panels |
| 4      | Single-axis tracking with double edge-to-edge spacing |
| 5      | Single-axis tracking with triple edge-to-edge spacing |
| 6      | Conventional fixed-tilt |
| 7      | Fixed-tilt with intermediate panel elevation |
| 8      | Fixed-tilt with elevated panels |
| 9      | Fixed-tilt with elevated and inter-row spaced panels |
| 10     | Fixed-tilt with double edge-to-edge spacing |
| 11     | Vertical mount |

### Variables

Here, gid indexes locations, time indexes hourly timestamps, and distance indexes inter-row sample points.

| Name | Description | Dimensionality | Units |
|:-----|:------------|:---------------|:------|
| albedo | Surface albedo (ground reflectivity) | (gid, time) | |
| annual_energy_per_acre | Annual energy production from a PV array on a 1-acre plot of land | (gid) | kWh/year/acre |
| annual_energy_per_kWdc_installed | Annual energy production per kW_DC installed PV | (gid) | kWh/year/kW_DC |
| annual_poa | Annual plane-of-array insolation | (gid) | Wh/m^2 |
| beda - bedf | Average ground irradiance across each of the planting beds defined in the inter-row spacing | (gid, time) | W/m^2 |
| celltemp | PV cell temperature | (gid, time) | °C |
| dc_gross_per_acre | Gross DC power production from the PV array, normalized by the land area of the array | (gid, time) | kW/acre |
| dc_gross_per_kWdc_installed | Gross DC power production from the PV array, normalized per kW_DC installed PV | (gid, time) | kW/kW_DC |
| dhi | Diffuse horizontal irradiance | (gid, time) | W/m^2 |
| dni | Direct normal irradiance | (gid, time) | W/m^2 |
| edgetoedge | Average ground irradiance in the edge-to-edge spacing between the PV panels | (gid, time) | W/m^2 |
| edgetoedge_par | Average photosynthetically active radiation in the edge-to-edge spacing between the PV panels | (gid, time) | umol/m2/s |
| farmable_land_percent | Percent of land area that is farmable, accounting for unfarmable area under the panels and 0.5 ft buffers around the panels | (gid) | % |
| ghi | Global horizontal irradiance | (gid, time) | W/m^2 |
| ground_irradiance | Irradiance that reaches the ground after shading from the PV array | (gid, time, distance) | W/m^2 |
| pitch | Row-to-row spacing | (gid) | m |
| poa_front | Plane-of-array irradiance on the front-side of the PV panels | (gid, time) | W/m^2 |
| poa_rear | Plane-of-array irradiance on the rear-side of the PV panels | (gid, time) | W/m^2 |
| relative_humidity | Relative humidity | (gid, time) | % |
| temp_air | Air temperature | (gid, time) | °C |
| tilt | Panel tilt angle, included only in the fixed-tilt and vertical configuration Zarr files. | (gid) | ° |
| underpanel | Average ground irradiance on the ground directly underneath PV panels | (gid, time) | W/m^2 |
| wind_direction | Wind direction | (gid, time) | ° |
| wind_speed | Wind speed | (gid, time) | m/s |

*All data variables are 64-bit floats (`float64`).*

*A note on dimensionality*:  
Variables with only `gid` dimension have one entry at each location.  
Variables with `gid` and `time` dimensions have one entry at each hour for each location.  
Variables with `gid`, `time`, and `distance` dimensions have one entry at 10 inter-row sample points for each hour at each location.

## Access Python Usage
This is both a set of functions as well as an exectuable python packge. It requires the passing of parameters at the start.
* system : Followed by an integer of the system unique ID. 
* path : Followed by a string that targets the local computer location/path you wish the data to be stored into.
* parquet : A flag that changes the data type from default CSV to Parquet file for download.

For using the functions, see the tutorial examples folder. There, the demo script for accessing agrivoltaics irradiance data from S3 zarr files can be run directly to see printed output
of all examples, or individual functions can be imported and used in other scripts

This script demonstrates three common use cases:
1. Loading data for selected setups by GID
2. Using the gid-lat-lon.csv lookup table to find the nearest GID for a given lat/lon
3. Accessing data for a lat/lon range

The data is stored in zarr format on S3 at:
    s3://oedi-data-lake/inspire/agrivoltaics_irradiance/v1.0/

Each setup has its own zarr file named: configuration_{setup:02d}.zarr

System IDs can be found in the metadata file for Available System Information on the [OEDI site](https://data.openei.org/submissions/4568)(Ref [1])</br> or from information on the [Agrivoltaics Shading Tool interactive website](https://openei.org/wiki/InSPIRE)(Ref [2]) 

### Processing
The Agrivoltaics Shading data will be downloaded as the series of timeseeries files. At this point you can plot ground irradiances for the full year, or use data processing to average or sum by day, month, season, or other metric of interest. 

## References
</code>
[1]:https://data.openei.org/submissions/4568
[2]:https://openei.org/wiki/InSPIRE
