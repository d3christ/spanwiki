Below you'll find the 2009 spanlab wiki entry for mask creation. 

If you're only interested in drawing a sphere at particular coordinate in TLRC space, see Nick's relevant python script in `spantoolbox/masks/make_masks.py`

# Group VOI Masks creation (2009)

Group masks are placed based on average anatomy (established Talairach coordinates). The same mask is used for all subjects. When placing the VOIs, consult previous spanlab research for the optimal coordinates. Since these masks are used for all the subjects, they must be created in +tlrc space.

* In the scripts directory of the study, copy in a sag116 file with seemingly normal anatomy to use as the underlay in drawing the masks. 

* Open AFNI.  Ensure that the 'View ULay Data Brick' is selected in the Define Datamode menu. Select the sag116 file as underlay and   switch to tlrc view. Under Define Datamode, click on write ULay. 

* Now, Define Datamode → Plugins → Drawdataset (which opens an 'AFNI Editor' window), then do the following:
 
* Make sure that the "Copy Dataset" box is checked and make sure that that "zero" is selected in the neighboring option box. This will ensure that you begin with a blank template. 

* Click on "Choose Dataset for copying". Choose sag116+tlrc and click "Set"

* The plugin will load the file as COPY_sag116+tlrc. In the main AFNI window, Make sure the underlay is set to sag116 and change the overlay to COPY_sag116+tlrc. Make  sure you are in tlrc view and make sure that "See Overlay" is selected.

* Next, see the [#Drawing Masks] section below

# Individual VOI Masks

Individual masks are placed based on the subject's unique anatomy so you will need to create a mask file for each participant. This is done in studies where brain anatomy is expected to differ (ex. Aging studies). Ideally, individual masks would be placed in original space to avoid an extra warping step. However, in terms of accuracy, individual masks should be placed in tlrc space. This is done to allow the use of specific canonical talairach coordinates as guides for the placement. For a given region, you would navigate to a specific reference coordinate for the VOI. Coordinates can be found in our lab's published works. You would then place the VOI in the ball park of your reference coordinate, insuring that the VOI is placed on grey matter.

* For individual masks, you will create the mask in the subject's directory and use the respective sag116 file for the subject. 

* Open AFNI. Ensure that the 'View ULay Data Brick' is selected in the Define Datamode menu. Select the sag116 file as underlay and switch to tlrc view. Under Define Datamode, click on    write ULay. 

* Now, Define Datamode → Plugins → Drawdataset (which opens an 'AFNI Editor' window), then do the following: 

* Make sure that the "Copy Dataset" box is checked and make sure that that "zero" is selected in the neighboring option box. This will ensure that you begin with a blank template. 

* Click on "Choose Dataset for copying". Choose sag116+tlrc and click "Set" 

* The plugin will load the file as COPY_sag116+tlrc. In the main AFNI window, Make sure the underlay is set to sag116 and change the overlay to COPY_sag116+tlrc. Make sure you are in tlrc view and make sure that "See Overlay" is selected. 

*Next, see the [#Drawing Masks] section below.

# Editing an existing mask:   

* Open AFNI from the directory that contains the mask for editing. Make sure the supporting sag116 file is also in the directory for underlay. Viewing space (orig or tlrc) will depend on the space of the file you are editing. Make sure you make all of your edits in the correct space. If you are editing an individual mask file you will likely need to work in +orig space. Conversely, canonical masks will need to be edited in +tlrc space. 

* Now, Define Datamode → Plugins → Drawdataset (which opens an 'AFNI Editor' window), then do the following:

* Make sure that the "Copy Dataset" box is checked and make sure that that "Data" is selected in the neighboring option box, instead of "zero". This will ensure that you begin with a copy of the original mask with the VOIs still included.

* Click on "Choose Dataset for copying." Choose the mask you want to edit and click "Set" 

* The plugin will load the file as COPY_*original mask file*. In the main AFNI window, Make sure the underlay is set to sag116 and change the overlay to the copy of the mask file. 

* If you cannot see the existing masks in the Overlay, you need to drop the threshold in Define Overlay all the way down. 

* Make sure that "See Overlay" is selected. Before editing the mask, make sure you know all the pre-existing indices. When you add VOIs to this mask you will need to assign an unused index.

If you are adding a new VOI to the mask file, see [#Drawing Masks] below. 

Removing a VOI from a mask is usually unnecessary unless you need to add another one in its exact location. Another option is to downsize the VOI mask you currently have. When altering a preexisting VOI within a maskfile, changes must be made slice by slice. Depending on what you need to do, it may be easier to just create another mask file. 
 
 Deleting Slice by Slice:
 * Set Mode: Flood>Zero
 * Set Value: 0 
 *  Shift-Click on the VOI slice
 *  This will remove all mask voxels in that slice. 
 * You can use this to delete a mask entirely by repeating the deletion slice by slice. 
 
 Deleting or Adding Part of a Mask within a Slice:
 * Set Mode: Filled Curve
 * Set Value: 0 (If adding, Set Value to the same as the VOI index you are adding to)
 * Shift+click to draw a loop around the area you want added or removed
 * Area within curve will be removed or filled with mask voxels depending on the Value set

# Drawing Masks

For ease of warping, it is HIGHLY recommended that you create one maskfile that contains all of your VOIs. If you do so, you will end up with a maskfile that has multiple VOIs that are differentiated by index number. You can double-check the index number in AFNI by clicking on a mask region and looking in expand Define Overlay, look at the OLay value in the Lower right-hand corner. Make sure to note the indices for your VOIs within the mask file. This will be needed when dumping data and if you ever need to add additional VOIs to the mask file. If you think you will have overlapping VOIs (i.e. you would like to test two mask sizes on the same region), you will need to create multiple mask files.

After following the instructions from one of the sections above, in the Draw Dataset Plugin:
Set these values:

'''
 Mode: 3D Sphere R = Radius; 4mm is normal
 Value: Starting from 1 for the first ROI  
 Label: Name of Region
'''

* Find the center of the region where you want to place the sphere (it is possible to zoom in and pan to get correct placement by using the buttons on the sides of the AFNI sagittal / coronal / axial views).  

* When you are centered, shift + click on the spot to place the sphere. 

* The overlay that you have copied now has a region of voxels with the value as defined in the draw dataset box. If you cannot see this sphere, you must go into Define Overlay and drop the threshold bar all the way to the bottom. 

* Increment the Value for your next VOI, Label the region, and repeat these steps until you’ve created all the ROIs you want.  
 
* If you would like to look at differences between hemispheres (ex. rNAcc vs. lNAcc), VOI masks will need separate spheres that are numbered differently . If you want to create a single bilateral mask for a VOI you can make a sphere on each side of the brain and give each sphere the same index number. When dumping out the data, the AFNI will treat this bilateral VOI as a single mask and average across hemispheres. 

When done, click the Save As button and name the mask. Since AFNI automatically copies any file you input into the Draw Dataset plugin. You will have COPY* files still in the directory. If you used Save As you can remove all of these COPY* files.
