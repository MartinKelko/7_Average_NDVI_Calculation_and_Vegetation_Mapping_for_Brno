# Average NDVI Raster Calculation

## Description

This script processes four Sentinel-2 NDVI (Normalized Difference Vegetation Index) GeoTIFF images for the city of Brno, Czechia. It calculates a new raster image representing the average NDVI values from all the input rasters. The script excludes -999 values, which typically indicate clouds, from the calculation. Additionally, the dataset includes Brno's boundaries in ESRI Shapefile format.

## How to Use

### Prerequisites

- Ensure you have ArcPy installed and licensed on your system.
- Prepare the working directory containing the input raster images and the ESRI Shapefile.

### Script Explanation

1. **Set Up the Environment**

   Define the workspace and enable output overwriting to ensure new outputs replace existing files.

   ```python
   import arcpy

   arcpy.env.workspace = r"C:\EsriTraining\WFS_interview\gis_star_task_01\gis_star_task_01\NDVI"
   arcpy.env.overwriteOutput = True

input_rasters = ["2023-03-03_09-57-06.tiff","2023-05-22_09-57-09.tiff","2023-08-15_09-57-10.tiff","2023-10-02_10-07-02.tiff"]
raster_list = []

for raster_file in input_rasters:
    raster = arcpy.Raster(raster_file)
    raster_list.append(raster)

def calc_average(*rasters):
    valid_rasters = [r for r in rasters if arcpy.Exists(r)] 
    valid_pixel_count = arcpy.sa.Con((valid_rasters[0] != -999), 1)  
    total_sum = arcpy.sa.CellStatistics(valid_rasters, "SUM", "DATA")
    return total_sum / valid_pixel_count

average_raster = calc_average(*raster_list)

output_path = r"C:\EsriTraining\WFS_interview\gis_star_task_01\gis_star_task_01\output_path/average_raster.tif"
average_raster.save(output_path)

print("Average raster saved to:", output_path)

python calculate_average_ndvi.py
