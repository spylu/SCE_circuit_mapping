# Instructions & Methods for Digitizing Circuit Boundaries

## Getting Started:
### Start with the Issues Tracker
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

The general workflow is as follows:
1.  Open Github and select a file from the list of files in the 'Issues' section of the page. Make sure to comment that you have started working on this pdf.
1.  Download the pdf you have selected, and isolate the single page labeled 'All Circuits'. The page is typically around page 10.
1.  (The following steps are described in more detail below)
1.  Take the map and import it into QGIS.
1.  'Pin' the map to georeference it in space.
1.  Draw the polygons for the circuits (aka, feeders).
1.  Add the attribute data for the polygon.
1.  Save the map and push it to the Github repository.

### Get the data/maps into QGIS
See above to get started. This section will detail the georeferencing portion of the workflow.

*** Fill this out as Michele describes how to do the task.***

### Make a boundary
The goal of delineating the circuits is for us to computationally connect a household/business to a specific circuit. To ensure we can do this we need to only allow for overlapping polygons in cases where there are two polygons that have different voltages.

### Update the attribute table

| Attribute Name | Type | Description | Example |
| --- | --- | --- | --- |
| PDF_name | text | Name of pdf | Adelanto |
| circuit_name | text | Circuit name | Caterpillar |
| confidence | integer | Confidence of delineation 1(low)-5(high) | | 
| percent_area | 0-100 | Percent area of circuit covered by another circuit |
| overlap_name | text | Overlapping circuit's name(s) | |
| edge | binary | Circuit on edge of map? | |
| connector | binary | Does a segment connect two polygons? | |
| name | text | Delineator's name | Ben Dawson |
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



