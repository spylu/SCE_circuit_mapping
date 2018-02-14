# Instructions & Methods for Digitizing Circuit Boundaries

## Getting Started:
### Start with the Issues Tracker

__This will be included once we are on the DEEP Github page__

1. In the Issues section of the SCE_circuit_mapping GitHub page, you should find an Issue for each map.  All maps in need of digitizing have an issue open on the github repository.  The issues are closed for maps as they are finished.  Maps are given labels to indicate their status.  Open issues labeled as "In Progress" are currently being digitized by a contributor.  Choose an map that does not have an "In Progress" label.  
2. Write a comment for your chosen map that you are working on the boundary.  If possible, assign the task to yourself (on the top of the right side panel of options) and add the "In Progress" label.  Add additional comments if any issues or questions arise about this particular map.


## Get the most recent project files from GitHub 
We’ll describe how to do this with the GitHub for Desktop tool, but you may use the tool of your choice.  We’ll also assume you’ve already set up your GitHub account and the GitHub for Desktop program.:
1.	Fork the SCE_circuit_mapping repository: https://github.com/BenMDawson/SCE_circuit_mapping .  Details about how to fork a repository and work with it in GitHub Desktop are here: https://guides.github.com/activities/forking/ 
1.	Open GitHub for Desktop
1.	Select the forked SCE_circuit_mapping repository on the left sides of the window.
1.	In your computer’s file navigation system, navigate to your GitHub folder and open the '2016_Data' folder. Within this folder are all of the pdfs that need to be geocoded. 

Once you've set up your fork, you'll need to update it regularly to make sure you have all the current files.  There is unfortunately no way to do this with the GitHub Desktop tool, but it's not too complicated to update it.
1. Open GitHub Desktop
1. Click on your fork to open it.
1. Right click on the name of the fork and select "Open Command Prompt" or "Open in Git Shell" (depending on the version you have the text will be different).  A command line shell will open.  The path before the > should be where you store your data (probably the GitHub folder on your computer).
1. You will now run a few commands to update your fork ([reference](https://gist.github.com/CristinaSolana/1885435)).
    1. The first time you'll need to set an upstream repository for your fork:
    ```
        git remote add upstream git://github.com/BenMDawson/SCE_circuit_mapping.git
    ```
        
    2. Now you'll fetch any changes:
    ```
        git fetch upstream
    ```
    
    3. Finally, you'll update your folder with the changes you just fetched:
    ```
        git pull upstream master
    ```

In the event that your fork gets too messy and merging pull requests or updating from the upstream branch gets complicated, you can do a **hard reset** to remove everything from your fork and replace it with what is on the UC Davis SCE_circuit_mapping repository. 
1. **Copy any data you've been working on into a folder not affected by git.** 
2. Run a few lines of code to reset your repository:
    1. The first time you'll need to set an upstream repository for your fork:
    ```
        git remote add upstream git://github.com/BenMDawson/SCE_circuit_mapping.git
    ```
    
    2. Now the reset:
    ```
    git fetch upstream
    git checkout master
    git reset --hard upstream/master  
    git push origin master --force
    ```
3. Finally, move any data you've been working on back into it's folder.  Now you can do a pull request like you normally would.

## Digitizing the Circuit Boundaries

__The general workflow is as follows__:
1.  Open Github and select a file from the list of files in the 'Issues' section of the page. Make sure to change the label once you have started working on this pdf.
1.  Download the pdf you have selected, and isolate the single page labeled 'All Circuits'. The page is typically around page 10.
1.  (The following steps are described in more detail below)
1.  Take the map and import it into QGIS.
1.  'Pin' the map to georeference it in space.
1.  Draw the polygons for the circuits (aka, feeders).
1.  Add the attribute data for the polygon.
1.  Save the map and push it to the Github repository.

### For the first time opening QGIS
Prior to starting work there need to be a few plugins installed in QGIS. Follow the direction below:
1.  Open QGIS
1.  In the menu go to: Plugins/Manage
1.  Search for and install: Georeference GDAL, QuickMapServices, and GeoJSONPlugin
1.  Once that is comple, go to the menu Web/QuickmapServices/Settings
    1.  Choose the 'More Services' tab
    1.  Select 'Get contributed pack'
1.  In the menu go to Web/QuickMapServices/Google/Google Road
    1.  'Ignore' if there is an error
1.  Now there should be a Google Map in the viewer
1.  Proceed to 'Get the data/maps into QGIS' below

### Get the data/maps into QGIS
*See above to get started if this is the first time you are opening QGIS. *

This section will detail the georeferencing portion of the workflow. This is a rough outline, and for details please refer to the tutorials in "Getting Started", i.e. [GettingStarted/Tutorials/QGIS/Georeferencing](https://github.com/BenMDawson/SCE_circuit_mapping/blob/master/GettingStarted.md)

1.  Open QGIS
    1.  Install plugins if you have not already - detailed above
1.  In the menu go to Web/QuickMapServices/Google/Google Road
    *   'Ignore' if there is an error
    *   Now there should be a Google Map in the viewer
1.  Go to the menu, Raster/Georeferencer/Georeferencer...
1.  In the upper-left corner go to 'Open Raster'
1.  Select the file to be rasterized
1.  Select Coordinate System: EPSG:3857
    *   This is the projection of the basemap that the pdf is 'pinned' onto.
    *   It can be searched for in the 'filter' field
1.  Find unique points (e.g. road intersections), select from the menu on the window 'Add Point', and then click on the map
1.  A dialogue box will appear and select 'From map canvas', this will take you back to the Google Maps base layer you originally opened. Select the same point you had just selected from the pdf image.
1.  Repeat these above two steps for 10-15 points spread out across the map - the further apart the better.
    *   *Note: Sometimes there are more or less information given how the data was digitized to ensure copyright, and so if there are differences, just find a point the is unique to both maps.*
1.  Once you have 10-15 points, in the top menu bar select Settings/Transformation Setings...
    *   Ideally once these points have been selected - look through all of the points and hopefully the 'residuals' will be around 1 (but maybe larger) - if they are extreme values 'turn them off'.
1.  Select 'Transformation Type': Linear and 'Resampling Method': Nearest Neighbor, and at the bottom select 'Load into QGIS when done'
1.  In the top left corner of the window select 'Start Georeferencing' and select Coordinate System: EPSG:3857
1.  On the main QGIS window should now lay your georeferenced map (over top the Google Maps base layer) for making boundaries

### Make a boundary
The goal of delineating the circuits is for us to computationally connect a household/business to a specific circuit. To ensure we can do this we need to only allow for overlapping polygons in cases where there are two polygons that have different voltages.

1.  With QGIS open and the georeferenced map, open via Layer/Add Layer/Add Vector Layer (or drag and drop) the 'template.geojson' file.
1.  Choose a circuit to delineate and select the 'Add Feature' button in the menu
    *   If this button is shaded out, click the 'Toggle Editing' button to the left of it
1.  Delineate the circuit - this is the key step of the process. The goal is to be able to match an address to a circuit, and thus to create a boundary between circuits such that from an address we know which house belongs to which circuit is tantamount.
    *   This requires judgement and pracice
    *   A good rule of thumb is to select the boundary that is exactly between the two neighboring circuits, but is not always the case
    *   If you reach a case that is unusual, please refer to the [unusual cases](https://github.com/BenMDawson/SCE_circuit_mapping/blob/master/Unusual_Cases.md) page
    *   If there is still ambiguity, please always feel free to e-mail me
1.  To do so, determine the polygon that best represents the circuit following the directions above
1.  The process started in step two, so at this point you can click around the boundary of the polygon to generate the new field
1.  If you like the delineation, right click, if you do not, press escape and start over at step 2.
1.  Repeat the above steps untill all of the circuits have been delineated
1.  Once all of circuits have been delineated select the 'template' polygon (the large square off of the shore of Long Beach) and delete that feature
    *   To do so, select the polygon and use the trash button in the menu
1.  SAVE! You're done.
1.  We need two files saved:
    1.  The full working file that you have just generated - this includes all of the layers, the georeferenced image, and all of the polygons just delineated. Save this file to the 'Archives' folder named as '(pdf name) _ archive.geojson' - there should be no spaces
        *   To save this, in the menu go to Projects/Save As
    1.  The polygon layer to the 'Completed' folder named as '(pdf name).geojson'
        *   To save this, right click on the template file and then Save As - be sure to rename the file for the appropriate pdf the file originated from
1.  Below (in the 'Save' section) details how to push the newly generated data to the Github page

### Update the attribute table

| Attribute Name | Type | Description | Example |
| --- | --- | --- | --- |
| pdf_name | text | Name of pdf | Adelanto |
| circuit_name | text | Circuit name | Caterpillar |
| confidence | integer | Confidence of delineation 1(low)-5(high) | | 
| percent_area | 0-100 | Percent area of circuit covered by another circuit |
| overlap_name | text | Overlapping circuit's name(s) | |
| edge | binary | Circuit on edge of map? | |
| connector | binary | Does a segment connect two polygons? | |
| delineator_name | text | Delineator's name | Ben Dawson |
| date | date | YYYY-MM-DD | 2018-01-01 |
| notes | text | Any extra notes | |

### Save

Once the circuits have been defined as polygons, save the data and push it to the Github repository.

## Submit your changes to the SCE_circuit_mapping GitHub Repository
1.	In GitHub for Desktop, you should see a list of changes you’ve made to the files.  Fill in the Summary and Description fields at the bottom of the window and then click the Commit button.  https://guides.github.com/activities/forking/#making-changes 
2.	If you are ready to incorporate your changes into the main branch, submit a pull request for your fork: https://help.github.com/articles/creating-a-pull-request-from-a-fork/ 
3.  If your changes are accepted, project administrators will incorporated your changes and close the issue for your map.  If there is any problems or questions, the project administrators will contact you.

-----------------------------------------------------------------------------

## Notes for pull request reviewers:
If a pull request cannot be merged automatically by GitHub, you can remove or modify files before you merge them into the main repository.

Step 1: In the command line tool, from your project repository, check out a new branch and test the changes.
```
git checkout -b [repository user name]-master master
git pull https://github.com/[repository user name]/[fork name].git master
```

Step 2: Remove or modify files on your computer.

Step 3: Commit the changes.

Step 4: Merge the changes and update on GitHub.
```
git checkout master
git merge --no-ff [repository user name]-master
git push origin master
```

Another Option: If you want to accept only some of the changes offered in a pull request, you will need to use the command line to [cherry-pick](https://mattstauffer.co/blog/how-to-merge-only-specific-commits-from-a-pull-request) the committs that you want to keep.

## Additional Reference Material:
1.	QGIS editing geometry manual: http://docs.qgis.org/2.14/en/docs/user_manual/working_with_vector/editing_geometry_attributes.html 
2.	Understanding the GitHub Flow: https://guides.github.com/introduction/flow/



