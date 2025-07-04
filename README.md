# Input data format for ESMValTool

## Background

[ESMValTool](https://esmvaltool.org/) is a community diagnostic and performance metrics tool for the evaluation and analysis of Earth system models.

A key focus is the joint analysis of a large variety of different data sets (e.g., climate model output from [CMIP](https://wcrp-cmip.org/) and [CORDEX](https://cordex.org/), observational data from [obs4MIPs](https://pcmdi.github.io/obs4MIPs/), other observational data, etc.). For this reason, it is essential that data sets from different sources are intercomparable. In practice, this is achieved with a common format standard that data sets need to follow.

## Recommended input data format

The preferred input file format for ESMValTool is [netCDF](https://www.unidata.ucar.edu/software/netcdf/). While ESMValTool can also read [GRIB](https://climatedataguide.ucar.edu/climate-tools/GRIB) files (and soon [Zarr](https://zarr.dev/) files), the vast majority of input data is in netCDF format. Thus, the following paragraphs specifically focus on the netCDF format.

Input data for ESMValTool need to be described by some sort of data specification (e.g., the [CMIP6 Data Request](https://clipc-services.ceda.ac.uk/dreq/index.html), the [CMIP7 Data Request](https://bit.ly/CMIP7-DReq-v1_2_1), the [obs4MIPs CMOR tables](https://github.com/PCMDI/obs4MIPs-cmor-tables), etc.) and formatted according to the [CF conventions](https://cfconventions.org/). A common tool to reformat native climate model output into CF-compliant netCDF files is the Climate Model Output Rewriter ([CMOR](https://cmor.llnl.gov/)). This reformatting is often reffered to as "*CMORizing*" and the resulting reformatted output as "*CMORized*" data.

For example, the data request may require that climate models need to output the 3D air temperature `ta`  with dimensions (time, pressure level, latitude, longitude) in monthly frequency for a historical simulation. The CF convenctions then describe how the actual data needs to be formatted, e.g., that there should be an attribute `units: K` and `standard_name: air_temperature` in the file.

<details>
<summary>Example of fully CF-compliant netCDF file (monthly mean air temperature on regular grid and pressure levels)</summary>

 [Table entry](https://github.com/PCMDI/cmip6-cmor-tables/blob/30dbad3ea0785c760c621b02185025c69df4d314/Tables/CMIP6_Amon.json#L1133-L1150)

```
netcdf ta_Amon_CanESM5_historical_r1i1p1f1_gn_185001-201412 {
dimensions:
        time = UNLIMITED ; // (1980 currently)
        bnds = 2 ;
        plev = 19 ;
        lat = 64 ;
        lon = 128 ;
variables:
        double time(time) ;
                time:bounds = "time_bnds" ;
                time:units = "days since 1850-01-01 0:0:0.0" ;
                time:calendar = "365_day" ;
                time:axis = "T" ;
                time:long_name = "time" ;
                time:standard_name = "time" ;
        double time_bnds(time, bnds) ;
        double plev(plev) ;
                plev:units = "Pa" ;
                plev:axis = "Z" ;
                plev:positive = "down" ;
                plev:long_name = "pressure" ;
                plev:standard_name = "air_pressure" ;
        double lat(lat) ;
                lat:bounds = "lat_bnds" ;
                lat:units = "degrees_north" ;
                lat:axis = "Y" ;
                lat:long_name = "Latitude" ;
                lat:standard_name = "latitude" ;
        double lat_bnds(lat, bnds) ;
        double lon(lon) ;
                lon:bounds = "lon_bnds" ;
                lon:units = "degrees_east" ;
                lon:axis = "X" ;
                lon:long_name = "Longitude" ;
                lon:standard_name = "longitude" ;
        double lon_bnds(lon, bnds) ;
        float ta(time, plev, lat, lon) ;
                ta:standard_name = "air_temperature" ;
                ta:long_name = "Air Temperature" ;
                ta:comment = "Air Temperature" ;
                ta:units = "K" ;
                ta:original_name = "TEMP" ;
                ta:cell_methods = "time: mean" ;
                ta:cell_measures = "area: areacella" ;
                ta:history = "2019-04-30T17:32:16Z altered by CMOR: Reordered dimensions, original order: lat lon plev time. 2019-04-30T17:32:16Z altered by CMOR: replaced missing value flag (1e+38) with standard missing value (1e+20)." ;
                ta:missing_value = 1.e+20f ;
                ta:_FillValue = 1.e+20f ;

// global attributes:
                :CCCma_model_hash = "3dedf95315d603326fde4f5340dc0519d80d10c0" ;
                :CCCma_parent_runid = "rc3-pictrl" ;
                :CCCma_pycmor_hash = "33c30511acc319a98240633965a04ca99c26427e" ;
                :CCCma_runid = "rc3.1-his01" ;
                :Conventions = "CF-1.7 CMIP-6.2" ;
                :YMDH_branch_time_in_child = "1850:01:01:00" ;
                :YMDH_branch_time_in_parent = "5201:01:01:00" ;
                :activity_id = "CMIP" ;
                :branch_method = "Spin-up documentation" ;
                :branch_time_in_child = 0. ;
                :branch_time_in_parent = 1223115. ;
                :contact = "ec.cccma.info-info.ccmac.ec@canada.ca" ;
                :creation_date = "2019-04-30T17:32:16Z" ;
                :data_specs_version = "01.00.29" ;
                :experiment = "all-forcing simulation of the recent past" ;
                :experiment_id = "historical" ;
                :external_variables = "areacella" ;
                :forcing_index = 1 ;
                :frequency = "mon" ;
                :further_info_url = "https://furtherinfo.es-doc.org/CMIP6.CCCma.CanESM5.historical.none.r1i1p1f1" ;
                :grid = "T63L49 native atmosphere, T63 Linear Gaussian Grid; 128 x 64 longitude/latitude; 49 levels; top level 1 hPa" ;
                :grid_label = "gn" ;
                :history = "2019-04-30T17:32:16Z ;rewrote data to be consistent with CMIP for variable ta found in table Amon.;\n",
                        "Output from $runid" ;
                :initialization_index = 1 ;
                :institution = "Canadian Centre for Climate Modelling and Analysis, Environment and Climate Change Canada, Victoria, BC V8P 5C2, Canada" ;
                :institution_id = "CCCma" ;
                :mip_era = "CMIP6" ;
                :nominal_resolution = "500 km" ;
                :parent_activity_id = "CMIP" ;
                :parent_experiment_id = "piControl" ;
                :parent_mip_era = "CMIP6" ;
                :parent_source_id = "CanESM5" ;
                :parent_time_units = "days since 1850-01-01 0:0:0.0" ;
                :parent_variant_label = "r1i1p1f1" ;
                :physics_index = 1 ;
                :product = "model-output" ;
                :realization_index = 1 ;
                :realm = "atmos" ;
                :references = "Geophysical Model Development Special issue on CanESM5 (https://www.geosci-model-dev.net/special_issues.html)" ;
                :source = "CanESM5 (2019): \n",
                        "aerosol: interactive\n",
                        "atmos: CanAM5 (T63L49 native atmosphere, T63 Linear Gaussian Grid; 128 x 64 longitude/latitude; 49 levels; top level 1 hPa)\n",
                        "atmosChem: specified oxidants for aerosols\n",
                        "land: CLASS3.6/CTEM1.2\n",
                        "landIce: specified ice sheets\n",
                        "ocean: NEMO3.4.1 (ORCA1 tripolar grid, 1 deg with refinement to 1/3 deg within 20 degrees of the equator; 361 x 290 longitude/latitude; 45 vertical levels; top grid cell 0-6.19 m)\n",
                        "ocnBgchem: Canadian Model of Ocean Carbon (CMOC); NPZD ecosystem with OMIP prescribed carbonate chemistry\n",
                        "seaIce: LIM2" ;
                :source_id = "CanESM5" ;
                :source_type = "AOGCM" ;
                :sub_experiment = "none" ;
                :sub_experiment_id = "none" ;
                :table_id = "Amon" ;
                :table_info = "Creation Date:(20 February 2019) MD5:374fbe5a2bcca535c40f7f23da271e49" ;
                :title = "CanESM5 output prepared for CMIP6" ;
                :tracking_id = "hdl:21.14100/f9e234d8-d1be-41ee-8ddc-4fcb9c7b8398" ;
                :variable_id = "ta" ;
                :variant_label = "r1i1p1f1" ;
                :version = "v20190429" ;
                :license = "CMIP6 model data produced by The Government of Canada (Canadian Centre for Climate Modelling and Analysis, Environment and Climate Change Canada) is licensed under a Creative Commons Attribution ShareAlike 4.0 International License (https://creativecommons.org/licenses). Consult https://pcmdi.llnl.gov/CMIP6/TermsOfUse for terms of use governing CMIP6 output, including citation requirements and proper acknowledgment. Further information about this data, including some limitations, can be found via the further_info_url (recorded as a global attribute in this file) and at https:///pcmdi.llnl.gov/. The data producers and data providers make no warranty, either express or implied, including, but not limited to, warranties of merchantability and fitness for a particular purpose. All liabilities arising from the supply of the information (including any liability arising in negligence) are excluded to the fullest extent permitted by law." ;
                :cmor_version = "3.4.0" ;
}
```

</details>

## Actually required input data format

It is highly recommended that all input data follow the format described above (i.e., the data should consist of CMORized netCDF files).

However, to improve user-friendliness, ESMValTool is lenient when it comes to the format of input data. Here is an overview of criteria that must or should be met, sorted by priority. Note that this list might not be complete.

### Absolutely mandatory criteria

*These criteria must be followed. Otherwise, data cannot be read with ESMValTool.*

1. netCDF variable names (including coordinates) must match the variable names defined by the data specificiation.
1. Coordinate variables for all coordinates required by the data specification must be present in the file. This also applies to [scalar coordinates](https://cfconventions.org/Data/cf-conventions/cf-conventions-1.12/cf-conventions.html#scalar-coordinate-variables). A netCDF variable is called "coordinate" if its name matches a netCDF dimension or if another variable includes its name in the attribute `coordinates` (see also [here](https://cfconventions.org/Data/cf-conventions/cf-conventions-1.12/cf-conventions.html#overview)).
1. All variables (including coordinates) must have the attribute `units`. Those units must be convertible to the units given in the data specification.
1. For coordinates other than time, latitude, and longitude, the attribute `standard_name` must be given.
1. For [parametric vertical coordinates](https://cfconventions.org/Data/cf-conventions/cf-conventions-1.12/cf-conventions.html#parametric-vertical-coordinate), the attributes `standard_name`, `formula_terms`, and `positive` must be given. All coordinates mentioned by the attribute `formula_terms` must be present in the file as netCDF coordinates.
1. Time units must be given as `[something convertible to seconds] since [reference datetime]` (see also [here](https://cfconventions.org/Data/cf-conventions/cf-conventions-1.12/cf-conventions.html#time-coordinate-units)).
1. If a netCDF variables uses fill/missing values different to the (data type-specific) defaults, the attributes `_FillValue` and/or `missing_value` must be given (see also [here](https://cfconventions.org/Data/cf-conventions/cf-conventions-1.12/cf-conventions.html#missing-data)). `NaN`s must not be used to indicate fill/missing values.

### Highly recommended criteria

*These criteria should be followed. While ESMValTool should be able to read data without following these criteria, unexcepted problems might occur.*

1. A single netCDF file should only include one variable including all its coordinates.
1. The file and/or directory naming convention should ensure that files can be uniquely identified by just looking at their file and/or directory names. Different facets in the file name should be separated by an underscore (`_`), and the file name should contain a time period identifier at the end of the filename that follows the [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format (if the variable is time dependent).
1. All netCDF variables should have the attributes `standard_name`, `long_name`, and `positive` (if applicable) as defined in the data specification.
1. If required by the data specification, coordinates should have bounds (identified by the attribute `bounds`).
1. The data type for coordinates should be `double` (i.e., double precision/float64); for all other variables it should be `float` (i.e., single precision/float32).
1. If required by the data specification, coordinates should have the requested values (mostly relevant for vertical coordinates).

### Recommended criteria

*These criteria can be followed. ESMValTool is able to read data without following these criteria*

1. Global netCDF attributes should be included with further information on the data (e.g., `source_id`, `experiment_id`, etc.).

### Examples


<details>
<summary>Minimal requirements for monthly mean precipitation (regular grid)</summary>

[Table entry](https://github.com/PCMDI/cmip6-cmor-tables/blob/30dbad3ea0785c760c621b02185025c69df4d314/Tables/CMIP6_Amon.json#L737-L754)

```
netcdf pr_Amon_CanESM5_historical_r1i1p1f1_gn_185001-185012 {
dimensions:
        time = 12 ;
        lat = 64 ;
        lon = 128 ;
variables:
        double time(time) ;
                time:units = "days since 1850-01-01 0:0:0.0" ;
        double lat(lat) ;
                lat:units = "degrees_north" ;
        double lon(lon) ;
                lon:units = "degrees_east" ;
        float pr(time, lat, lon) ;
                pr:units = "kg m-2 s-1" ;
}
```


</details>

<details>
<summary>Minimal requirements for monthly mean near-surface air temperature (regular grid including scalar height coordinate)</summary>

[Table entry](https://github.com/PCMDI/cmip6-cmor-tables/blob/30dbad3ea0785c760c621b02185025c69df4d314/Tables/CMIP6_Amon.json#L1151-L1168)

```
netcdf tas_Amon_CanESM5_historical_r1i1p1f1_gn_185001-185012 {
dimensions:
        time = 12 ;
        lat = 64 ;
        lon = 128 ;
variables:
        double time(time) ;
                time:units = "days since 1850-01-01 0:0:0.0" ;
        double lat(lat) ;
                lat:units = "degrees_north" ;
        double lon(lon) ;
                lon:units = "degrees_east" ;
        double height ;
                height:units = "m" ;
        float tas(time, lat, lon) ;
                tas:coordinates = "height" ;
                tas:units = "K" ;
}
```

</details>

<details>
<summary>Minimal requirements for monthly mean air temperature (regular grid on pressure levels)</summary>

[Table entry](https://github.com/PCMDI/cmip6-cmor-tables/blob/30dbad3ea0785c760c621b02185025c69df4d314/Tables/CMIP6_Amon.json#L1133-L1150)

```
netcdf ta_Amon_CanESM5_historical_r1i1p1f1_gn_185001-185012 {
dimensions:
        time = 12 ;
        plev = 19 ;
        lat = 64 ;
        lon = 128 ;
variables:
        double time(time) ;
                time:units = "days since 1850-01-01 0:0:0.0" ;
        double plev(plev) ;
                plev:units = "Pa" ;
                plev:standard_name = "air_pressure" ;
        double lat(lat) ;
                lat:units = "degrees_north" ;
        double lon(lon) ;
                lon:units = "degrees_east" ;
        float ta(time, plev, lat, lon) ;
                ta:units = "K" ;
}
```

</details>

<details>
<summary>Minimal requirements for monthly mean O3 mole fraction (regular grid on hybrid pressure levels)</summary>

[Table entry](https://github.com/PCMDI/cmip6-cmor-tables/blob/30dbad3ea0785c760c621b02185025c69df4d314/Tables/CMIP6_AERmon.json#L1349-L1366)

```
netcdf o3_AERmon_CanESM5_historical_r1i1p1f1_gn_185001-185012 {
dimensions:
        time = 12 ;
        lev = 40 ;
        lat = 90 ;
        lon = 144 ;
variables:
        double time(time) ;
                time:units = "days since 1850-1-1" ;
        double lev(lev) ;
                lev:units = "1" ;
                lev:standard_name = "atmosphere_hybrid_sigma_pressure_coordinate" ;
                lev:formula_terms = "p0: p0 a: a b: b ps: ps" ;
                lev:positive = "down" ;
        double p0 ;
                p0:units = "Pa" ;
        double a(lev) ;
        double b(lev) ;
        float ps(time, lat, lon) ;
                ps:units = "Pa" ;
        double lat(lat) ;
                lat:units = "degrees_north" ;
        double lon(lon) ;
                lon:units = "degrees_east" ;
        float o3(time, lev, lat, lon) ;
                o3:units = "mol mol-1" ;
}
```

</details>

<details>
<summary>Minimal requirements for monthly mean sea surface temperature (curvilinear grid)</summary>

[Table entry](https://github.com/PCMDI/cmip6-cmor-tables/blob/30dbad3ea0785c760c621b02185025c69df4d314/Tables/CMIP6_Omon.json#L4625-L4642)

```
netcdf tos_Omon_CanESM5_historical_r1i1p1f1_gn_185001-185012 {
dimensions:
        time = 12 ;
        j = 291 ;
        i = 360 ;
variables:
        double time(time) ;
                time:units = "days since 1850-01-01 0:0:0.0" ;
        double lat(j, i) ;
                lat:units = "degrees_north" ;
        double lon(j, i) ;
                lon:units = "degrees_east" ;
        float tos(time, j, i) ;
                tos:coordinates = "lat lon" ;
                tos:units = "degC" ;
}
```

</details>

<details>
<summary>Minimal requirements for monthly mean sea water salinity at ocean floor (unstructured grid)</summary>

[Table entry](https://github.com/PCMDI/cmip6-cmor-tables/blob/30dbad3ea0785c760c621b02185025c69df4d314/Tables/CMIP6_Omon.json#L4265-L4282)

```
netcdf sob_Omon_CanESM5_historical_r1i1p1f1_gn_185001-185012 {
dimensions:
        time = 12 ;
        ncells = 830305 ;
variables:
        double time(time) ;
                time:units = "days since 1850-1-1 00:00:00" ;
        float sob(time, ncells) ;
                sob:coordinates = "lat lon" ;
                sob:units = "0.001" ;
        double lat(ncells) ;
                lat:units = "degrees_north" ;
        double lon(ncells) ;
                lon:units = "degrees_east" ;
}
```

</details>

### Additional notes

1. If a variable is not available with the desired frequency in the data specification, the frequency can be overwritten using the facet `frequency` in data set definition in the [recipe](https://docs.esmvaltool.org/projects/ESMValCore/en/latest/recipe/overview.html) (see example [here](https://github.com/ESMValGroup/ESMValTool/blob/89fc58b0308a18c1ba5a5ec1422db926f2f74860/esmvaltool/recipes/examples/recipe_check_obs.yml#L248)).
1. If a variable is not available at all in the data specification, a [custom table](https://docs.esmvaltool.org/projects/ESMValCore/en/latest/quickstart/configure.html#custom-cmor-tables) can be defined for it.
