# TGIF: Copernicus Climate Data Store

When: 05.11.2021

## Copernicus: Climate Data Store 

https://cds.climate.copernicus.eu


## Part 1: Overview on Copernicus Climate Data Store

### Slides on the C3S CDS

[Slides](https://docs.google.com/presentation/d/1MmbCbASpAJJq5yUgcVXKhNwZTvrRwo_N/edit?usp=sharing&ouid=102118564710801685708&rtpof=true&sd=true)


## Part 2: Live Demo

You need to register and login in Climate Data Store:
https://cds.climate.copernicus.eu/

### Download CMIP6 Datasets

Search Datasets for "CMIP6":
https://cds.climate.copernicus.eu/cdsapp#!/search?type=dataset

The will let to this download formular:
https://cds.climate.copernicus.eu/cdsapp#!/dataset/projections-cmip6?tab=form

Search:
* Variable: Near-surface air temperature
* Model: MPI-ESM1-2-HR (Germany)
* Time: 2000-01-01/2000-01-31
* Area (Africa): -40W,70E,70N,-40S

Submit Formular.

This will open running requests:
https://cds.climate.copernicus.eu/cdsapp#!/yourrequests

### Use Toolbox Editor

The Toolbox Editor provides a simliar environment like Jupyter Notebooks.
But the toolbox is only available in the Climate Data Store.
There are plans to provide a toolbox api to be used in Jupyter Notebooks.

Use the same dataset search as above.

Use the button *Show Toolbox request* and copy the data request:
```
data = ct.catalogue.retrieve(
        'projections-cmip6',
        {
            'temporal_resolution': 'monthly',
            'experiment': 'historical',
            'level': 'single_levels',
            'variable': 'near_surface_air_temperature',
            'model': 'mpi_esm1_2_hr',
            'date': '2000-01-01/2000-01-31',
            'area': [
                70, -40, -40,
                70,
            ],
        }
    )
```

Open the toolbox editor:
https://cds.climate.copernicus.eu/cdsapp#!/toolbox

Load or create the application to plot the CMIP6 data with the copied data request:
```
import cdstoolbox as ct

@ct.application(title='Plot CMIP6')
@ct.output.figure()
def plot_cmip6():
    data = ct.catalogue.retrieve(
        'projections-cmip6',
        {
            'temporal_resolution': 'monthly',
            'experiment': 'historical',
            'level': 'single_levels',
            'variable': 'near_surface_air_temperature',
            'model': 'mpi_esm1_2_hr',
            'date': '2000-01-01/2000-01-31',
            'area': [
                70, -40, -40,
                70,
            ],
        }
    )
    fig = ct.map.plot(data, title="CMIP6 Plot")
    return fig
```

Run the application to generate the plot.


### Use CDS API

The [cdsapi](https://pypi.org/project/cdsapi/) is a Python library to download data from the Climate Data Store.

You need an access key. Read the `cdsapi` documentation how to get the key and configure it.

Use the same dataset search as above.

Use the button *Show API request* and copy the data request
```
c.retrieve(
    'projections-cmip6',
    {
        'temporal_resolution': 'monthly',
        'experiment': 'historical',
        'level': 'single_levels',
        'variable': 'near_surface_air_temperature',
        'model': 'mpi_esm1_2_hr',
        'date': '2000-01-01/2000-01-31',
        'area': [
            70, -40, -40,
            70,
        ],
        'format': 'zip',
    },
    'download.zip')
```

Open the notebook `notebook/cdsapi.py` and replace the data request.

Run the notebook.

### Use Rooki

[Rooki](https://github.com/roocs/rooki) is a Python client to interact with [Rook](https://github.com/roocs/rook) data reduction service for climate model data. This service is used in the backend by the Climate Data Store to access the CMIP6 data pool. The Rook service is deployed for load-balancing at CEDA (UK), IPSL (FR) and DKRZ (DE).

Open the notebook `notebooks/rooki.ipynb`.

Run the notebook.