# "end to end" tools for NGIAB data preparation
Quick disclaimer: This has had very little polish and I expect it to be rearranged shortly. see issues.  
The code needs a refactor to make it easier to understand, adapt, and reuse elsewhere.


## Aproximate workflow
1) Select the water basins you're interested in on the map
1) Click subset, this creates a geopackage with the waterbains you've selected + any basin upstream of it
1) pick a date and time
1) generate forcings for your water basins 

# Using this tool
*For forcing generation you need to install exact_extract too, see below

automation of this bit coming soon™
```bash
# needs ogr2ogr command to subset
sudo apt install gdal-bin
git clone https://github.com/JoshCu/NGIAB_data_preprocess
# git clone git@github.com:JoshCu/NGIAB_data_preprocess
cd NGIAB_data_preprocess
python -m venv env
source env/bin/activate
pip install -r requirements.txt
# once you download these two files, you can subset quickly to your hearts content
# but it may take a while to download
cd data_sources
wget https://lynker-spatial.s3.amazonaws.com/v20.1/conus.gpkg
wget https://lynker-spatial.s3.amazonaws.com/v20.1/model_attributes.parquet
cd ../map_app
flask -A __init__ run --debug
```
#### When using the tool, the map will tell you what folder it subset to inside the output folder in the root of this repo. The naming needs fixing and currently is just whatever waterbasin in the selection comes first alphabetically.
*THERE IS NO OVERWRITE PROTECTION ON THE FOLDERS*
## Forcings generation uses exact_extract
Full documentation [here](https://github.com/isciences/exactextract/tree/master/python)  
On ubuntu 22.04, there's a package for GEOS, if you can't find one then [build from source :\( ](https://github.com/libgeos/geos/blob/main/INSTALL.md])
```bash
# assuming you just did the block above and are in the map_app dir
cd ../..
pip install "pybind11[global]"
sudo apt install libgeos3.10.2 # possibly libgeos-c1v5 too
git clone https://github.com/isciences/exactextract/tree/master
cd exactextract/python
pip install .
cd ../../NGIAB_data_preprocess/map_app
flask -A __init__ run --debug
```