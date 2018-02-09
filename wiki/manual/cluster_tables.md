# Cluster tables
Usually when writing papers, you will wish to make a table consisting of clusters of activation in whole brain maps. These are normally organized like so:

```
TTRegion	                        X	 Y	 Z	 zScore	 Voxels
Right Lentiform Nucleus         	16	7	-3	 7.3315	 12937
Left  Lingual Gyrus             	-13	-86	-9	4.9527	326
Right Cerebellar Tonsil         	42	-54	-41	5.9384	183
Right Superior Temporal Gyrus   	54	-57	26	5.1105	142
```

Where zScore is usually the peak value and voxels is cluster size. We have automated this process. 

If you only wish to get one of the tables as above, you can use the `tableDump.py` and `cluster_automator.py` scripts originally by Andrew Trujillo. They are currently accessible in `spantoolbox/group_analysis/old_table_dump`

A more recent version of the script exists as clust2table.py

This will not print a finalized table as above, but instead is designed to give you more information about the clusters you have by giving you:
* Multiple candidate region names, for both CM and Peak
* Neurosynth associated terms for each cluster
* Coordinates in both MNI and TLRC space. 

# Using clust2table.py

The current version of the script is located in `spantoolbox/group_analysis/clust2table.py

### Requirements:
* You must have python installed
* You need to have pip packages selenium, pandas, and lxml installed (command: `pip install [package]`) 
* You will need to have a local version of Chrome and [chromedriver](https://sites.google.com/a/chromium.org/chromedriver/downloads) that you can point the script to. This lets us scrape Neurosynth. 

### Usage:
* Make sure `DRIVER_PATH` at line 24 points to your local copy of the driver. 
* Then you'll need to edit these lines:
```
    map_dir = os.getcwd() # By default this gets the current directory. 
    
    # map names should correspond to files ending in +tlrc.BRIK/HEAD in map_dir
    maps = ['inflection_z']

    #Create the table object. PASS False if you don't want to get the neurosynth associations (as this takes time).
    ct = ClusterTable(maps, map_dir, neurosynth=True)

    # generate the clusters
    z_value = 2.805 # for 2 tailed p < .005, use 3.3 for < .001
    ct.gen_clusters(z_value, write_1D=False) 

    # specify the output format: this is the mapping between vars and csv column
    # names. Comment out lines you don't want and change the right side to change
    # the csv column name (obviously don't use commas). Leave the left as is.

    header_format = OrderedDict([
        ('cm_names' , 'TT_Regions_CM'),        # tlrc map names for cm,  ; separated
        ('peak_names', 'TT_Regions_Peak'),     #     ""    ""   for peak ; separated
        ('peak_mnic' , 'PMNI_X,PMNI_Y,PMNI_Z'), # MNI coords RAI for peak, x, y, z
        ('peak_ttc' , 'PTT_X,PTT_Y,PTT_Z'),    # tlrc coords RAI for peak
        ('cm_mnic'  , 'CMNI_X,CMNI_Y,CMNI_Z'),  # MNI coords RAI for cm
        ('cm_ttc'    , 'CMTT_X,CM_TTY,CM_TTZ'), # tlrc coords RAI for cm
        ('volume'   , 'Voxels'),                # size in voxels of cluster,
        ('zScore'   , "Peak_zScore"),           # Z_score (i.e. the peak value, max_int in 3dclust)
        ('neurosynth_associations' , ','.join(['NS_' + str(x) for x in range(1,11)]))  # NS_1,...,NS_10, for the peak
    ])

    #write the csvs
    ct.write_csvs(header_format)
```


