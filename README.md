**PIV for Alaskan River Change Workflow**

In order to generate detailed pixel-by-pixel information on change over time, we use Chadwick et al’s (2022) technique for mapping and analyzing river migration, which utilizes a process called Particle Image Velocimetry (PIV) to indicate channel change over time. It reduces temporal pairs of images into vectors indicating direction and rate of change. This implementation is intended for to annually update the existing PIV dataset of the Kanektok River, but can conceivably be used for an entirely different river.

**Required Software**

1. ESRI ArcGIS Pro
2. Anaconda
3. An internet connection and browser of choice
4. (OptionaL) Matlab desktop, otherwise Matlab free browser version is fine

**STEP 1: Generate and Download New Yearly Composite**
*For this step, you need to make a Google Earth Engine account, and install a copy of ArcGIS Pro*

1. Create a Google Earth Engine account and paste the following script into the analysis window. it is file S1 in the GitHub repository https://github.com/JonLim20/CIVIC/blob/4ffc068e22a20070730105e65aa133259a7afd39/LANDSAT8%20Google%20Earth%20Engine
2. Adjust date range and study area, then press run to preview the imagery. 
3. If imagery looks good, download the imagery for the year you are interested in, bring it into ArcGIS Pro, and then create binary planforms using Step 2 below
4. All available raw imagery dating to 1999-2025 is archived in the Release panel on the right.

**STEP 2: Binary Thresholding Workflow**

This is the workflow for creating river planform binary rasters in ArcGIS Pro. I have split it into two manual sections and two automated sections.

*Section 1: Manual*

1. Create a new project in ArcGIS Pro.  
2. Download the imagery from the ArcGIS Online group. At the moment I have it set up as a map package but I might change that later  
   1. Select image, correctly assign bands and adjust contrast  
   2. Make sure crs of project is in WGS 1984 (A geographic coordinate reference system which is native to the imagery  
   3. Save it into your project folder, normall in My Documents→ ArcGIS→Projects→ \*Name\*. It can be opened within arcgis pro’s catalog pane, or simply drag it into an open map to load it.  
3. Download the Binary Raster Automated Sections.ipynb from GitHub. Place it in the project folder as stated in section 2c) above  
4. Isolate the near infrared band to make new layer. This is the only way to allow you to adjust the symbology using the Classified option.  
   1.  Select the image in the contents pane→ Imagery tab→ Raster functions→ search the raster function window for Extract Bands→ Select the raster in the drop down menu under “Raster”. State the single band in question in the “Combination” box→ “Create new layer”  
5. Initial thresholding to create binary image, representing water and land  
   1. Right click on extracted layer in the contents pane to the left→symbology→Change primary symbology to classify→Classes to 2, play with the lower number of the upper values.   
   2. We want water feature to be within 1 pixel of water (30m)

*Section 2: Automated*

6. Open the “Binary Raster Automated Sections.ipynb” script from the catalog pane to the right. Right click on it→ open. **Refer to Module 1**, fill in the relevant details in Parts 1-3 and run the first cell only:  
   1. Adjust directory, make file for binary output  
   2. Fill in landcover value thresholds from extract bands label  
   3. Make sure band set to near infrared  
   4. This will output an initial binary raster called “shapefiletemp’ and a StudyArea file (Previously known as the “blob”)

*Section 3: Manual*

7. Isolate the Kanektok shape on the ‘shapefiletemp’ vectorized image  
   1. Map, select, hold shift and click on water, continue until all river is highlighted  
   2. After selected, right click the vector layer, data, export features, export selected records only, title it “IRS\_\*year\*” (which stands for isolated river shapefile) or similar.  

*Section 4: Automated*

10. Run Module 2 only, of the automated workflow. This will generate the final products. In this module, fill in parts 1-3 to direct the script to the dissolved IRS.

[^1]:  https://pro.arcgis.com/en/pro-app/latest/help/editing/merge-features-into-one-feature.htm

[^2]:  https://pro.arcgis.com/en/pro-app/latest/tool-reference/data-management/create-fishnet.htm

[^3]:  https://pro.arcgis.com/en/pro-app/3.4/tool-reference/data-management/dissolve.htm

Make 4 realizations, each with different definitions of water.

**Step 3: Clipping the products**

9.	Generate a study area and then export it to a folder as a shapefile (ie. Not within a geodatabase). Take care to save the .prj and .tfw for use in Step 5
10.	 Clip the geometry using the script provided, to reduce the amount of area being analyzed. It also reprojects the 
11.	 All available unclipped and clipped binary planforms are archived in the PIV Data folder, under the images_unclipped and images_clipped folder respectively

**Step 4: PIV**

13.	Using the settings outlined in Chadwick et al (2023), carry out PIV on pairs of years for each visualization (ie. 8 realizations per pair)
i.	Interrogation area pass 1 = size of widest channel in pixels. For Kanektok, 32 pixels, step 16
ii.	Pass 2 and pass 3 half of each other. For Landsat, 16 and 8pixels.
iii.	Correllation robustness= extreme


**Step 5: Matlab**

15.	Create an account in Matlab Online, and paste the provided script there, in order to process the realizations.
16.	Create a folder in Matlab Online, and upload the realizations by pasting it into the folder
17.	Modify the settings as needed in the provided script (S4) and run the realizations, including the .prj and .tfw files produced in Step 3
18.	This outputs the results as vector confidence graphs and the vectors themselves as shapefiles, embedded with magnitude of movement over time
19.	All available vector shapefiles and PIV pairs calculate are archived in the PIV Data folder, under the "pivpairs"  folder

**Step 6: Carry out spatial join, merge tables, and generate heatmap of change**

20. Carry out a spatial join between the vectors and the corresponding sectors (Sector, in the PIV Data folder) they are in
21. Export these as .csv files to a folder containing all other spatial-joined vector .csvs
22. Execute script S5 in python, after directing it to the relevant folder. 

