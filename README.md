**WARNING**: *This tool has not yet been optimized for public use. Much of the logic is hardcoded to Shelby County, TN data. Shelby County 9-1-1 assumes no responsibility for any inaccurate logic therein contained.*

# Introduction

This FME tool imports GIS parcel files and a comma-delimited plain text file containing the official certified parcel attribution. The purpose of this tool is to add this attribution to the GIS parcel files. The attribution added is cleaned up prior to the the tool writing any outputs.

This tool was originally intended for Shelby County, TN. For official parcel information in Shelby County, see the Shelby County Assessor's [website](https://www.assessormelvinburgess.com/welcome). Please note that this URL may change depending on thew results of local elections.

## Requirements
* FME Form 2025.2 or newer (older versions may work as well, but are not guaranteet to work)
  * ...or equivalent in Esri's Data Interoperability extension
* Geospatial parcels file (shapefile, file geodatabase, etc.) containing the spatial representation of the parcels. The file/layer must contain a common field with the non-spatial structured file (parcel id, etc.).
* Non-spatial structured file (.txt, .csv, etc.) containing the official certified parcel attribution. The file must contain a common field with the geospatial parcels file (parcel id, etc.).

## Technical Details
### Imports and Exports
This toolset has 3 data dependencies that must be set prior to running the tool and imported via readers:
1. Geospatial parcels file (shapefile, file geodatabase, etc.) containing the spatial representation of the parcels. The file/layer must contain a common field with the non-spatial structured file (parcel id, etc.).
   * The tool expects this file/layer to be in a file geodatabase by default, but it can be in any format that FME supports. Update the reader types/parameters as needed.
2. Non-spatial structured file (.txt, .csv, etc.) containing the official certified parcel attribution. The file must contain a common field with the geospatial parcels file (parcel id, etc.).
   * The tool expects this file to be a comma-delimited plain text file by default, but it can be in any format that FME supports. Update the reader types/parameters as needed.
3. Output geospatial parcels file (shapefile, file geodatabase, etc.) containing the spatial representation of the parcels with the official certified parcel attribution added.
   * This file/layer is used for writing the output of the tool.
   * The tool expects this file/layer to be in a file geodatabase by default, but it can be in any format that FME supports. Update the writer types/parameters as needed.

### Logic
The tool performs the following general steps:
1. Reads in the geospatial parcels file and the non-spatial structured file.
2. Cleans up the attribution in the non-spatial structured file as needed.
3. Cleans up the attribution and geometry of the geospatial parcels file as needed.
4. Joins the cleaned non-spatial structured file to the cleaned geospatial parcels file based on the common field (parcel id, etc.).
5. Writes the output geospatial parcels file with the official certified parcel attribution added.
6. [Pending] Writes a log file containing any errors or warnings encountered during the process.
7. [Pending] Writes a report file containing a summary of the process, including the number of records processed, the number of records successfully joined, and any errors or warnings encountered.
8. [Pending] Sends a notification (email, etc.) upon completion of the process, including a summary of the results and any errors or warnings encountered.

### Important Notes
* This toolset has extensive use of regex for cleaning up the attribution in the non-spatial structured file and the geospatial parcels file. The regex patterns used are specific to the formatting of the data in Shelby County, TN. If adapting this tool for a different jurisdiction, these regex patterns may need to be adjusted to match the formatting of that data.
* The toolset also has extensive use of hardcoded field names and values that are specific to the data in Shelby County, TN. If adapting this tool for a different jurisdiction, these field names and values may need to be adjusted to match the data in that jurisdiction.
* The toolset is not optimized for performance and may take a long time to run, especially with large datasets. If adapting this tool for a different jurisdiction, consider optimizing the logic for performance as needed.
  * This may include removing unnecessary transformers, optimizing regex patterns, and/or using more efficient transformers for certain tasks (e.g., using a database join instead of an in-memory join for large datasets).