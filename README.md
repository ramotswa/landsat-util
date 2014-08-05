landsat-util
============

A utility to search, download and process Landsat 8 satellite imagery

*__Important:__ This repo is under development and not ready for deployment yet.*

## Install Dependencies

#### On Mac

If you don't have brew on Mac, go [here](http://brew.sh/).

    $: brew udpate
    $: brew install gdal

To install virtualenv run:

    $: sudo easy_install virtualenv

To make and activate virtualenv

    $: virtualenv name_of_the_environment
    $: source name_of_the_environment/bin/activate

Read here for [more info](https://gist.github.com/scisco/7485a7b9fac30be164c0)

#### On Ubuntu (Tested on Ubuntu 14.04)

Install PIP and some other  dependencies for a successful install of requirements.txt

    $ sudo apt-add-repository ppa:ubuntugis/ubuntugis-unstable
    $ sudo apt-get update
    $: sudo apt-get install python-pip build-essential libssl-dev libffi-dev python-dev python-gdal -y

To Run the API [read this](https://github.com/developmentseed/landsat-util/tree/master/api).

## Landsat8 Utility

    $: git clone https://github.com/developmentseed/landsat-util.git
    $: cd landsat-util
    $: pip install -r requirements.txt

```
Usage: landsat_util.py [options]

Options:
  -h, --help            show this help message and exit
  --search_array="path,row,path,row, ... "
                        Include a search array in this format:
                        "path,row,path,row, ... "
  --start=01/27/2014    Start Date - Format: MM/DD/YYYY
  --end=02/27/2014      End Date - Format: MM/DD/YYYY
  --shapefile=my_shapefile.shp
                        Generate rows and paths from a shapefile. You must
                        create a folder called 'shapefile_input'. You must add
                        your shapefile to this folder.
  --country=Italy       Enter country NAME or CODE that will designate
                        imagery area, for a list of country syntax visit
                        ("https://docs.google.com/spreadsheets/d
                        /1CgC0rrvvT8uF9dgeNMI0CVVqc0z85N-
                        K9cEVnN01aN8/edit?usp=sharing)"
  --update-metadata     Update ElasticSearch Metadata. Requires access
                        to an Elastic Search instance
```

For example

    $: ./landsat_util.py --search_array "166,036,166,037,166,038,166,039" --start "07/01/2014" --end "08/01/2014"
    $: ./landsat_util.py --country=Italy
    $: ./landsat_util.py --shapefile=custom-test.shp

Make sure to use right format for rows and paths. For example instead of using `3` use `003`.

The query string looks at the information included in the image name. Here is how the convention works:

### Naming conventions for Landsat scene identifiers

**LXSPPPRRRYYYYDDDGSIVV**
```
L = Landsat
X = Sensor
S = Satellite
PPP = WRS path
RRR = WRS row
YYYY = Year
DDD = Julian day of year
GSI = Ground station identifier
VV = Archive version number
```

**Examples:**
```
LC80390222013076EDC00 (Landsat 8 OLI and TIRS)
LO80390222013076EDC00 (Landsat 8 OLI only)
LT80390222013076EDC00 (Landsat 8 TIRS only)
LE70160392004262EDC02 (Landsat 7 ETM+)
LT40170361982320XXX08 (Landsat 4 TM)
LM10170391976031AAA01 (Landsat 1 MSS)
```

### Output folder structure

When you run the program an output folder will be created on the main folder

```
.
|-- ds-imagery
|     |-- output
|     |   |-- imagery
|     |   |   |-- file_scene
|     |   |   |-- zip
|     |   |   |   |-- LC80030032014174LGN00.tar.bz
|     |   |   |-- unzip
|     |   |   |   |-- LC80030032014174LGN00
|     |   |   |   |-- LC80030032014174LGN00_B1.TIF
|     |   |   |   |-- LC80030032014174LGN00_B2.TIF
|     |   |   |   |-- LC80030032014174LGN00_B3.TIF
|     |   |   |   |-- LC80030032014174LGN00_B4.TIF
|     |   |   |     |-- LC80030032014174LGN00_MTL.txt
|     |   |-- Shapefiles
|     |   |   |-- input
|     |   |   |-- output

```
