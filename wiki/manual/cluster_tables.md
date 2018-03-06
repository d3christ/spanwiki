# Cluster tables
When writing papers, you may wish to make a table consisting of clusters of activation in whole brain maps (as below). 
```
TTRegion	                        X	 Y	 Z	 zScore	 Voxels
Right Lentiform Nucleus         	16	7	-3	 7.3315	 12937
Left  Lingual Gyrus             	-13	-86	-9	4.9527	326
Right Cerebellar Tonsil         	42	-54	-41	5.9384	183
Right Superior Temporal Gyrus   	54	-57	26	5.1105	142
```
Where zScore is usually the peak value and voxels is cluster size.

To produce these tables, use (tools accessible in `spantoolbox/group_analysis/old_table_dump` on uncus.stanford.edu):
1. 3dclustsim to determine which cluster and threshold to use (TBA)
2. `cluster_automator.py` (which calls `tableDump.py`) to dump out the table with relevant information. Instructions for `cluster_automator.py`... 
* edit the `ttest_dirs` in the main loop to input the ttest directories for which you want tables
* edit the `fid_write` line in `write_clustcommands()` function with the correct threshold and cluster size (note that the two thresholds come after the `-2thresh` flag and the cluster size is the third argument after the `-dxyz` flag)
* comment out the two `run_*` commands in the main script (leaving the `write_*` command uncommented), and run `python cluster_automator.py` to output `clustcommand` in each ttest directory
* `cd` into each ttest directory and `chmod +x clustcommand`
* comment out the `write_*` command in the main script (leaving the two `run_*` commands uncommented), and run `python cluster_automator.py` to output the `*.csv` files for each regressor of interest

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

This will result in a csv file such as the following:

```
TT_Regions_CM,TT_Regions_Peak,PMNI_X,PMNI_Y,PMNI_Z,PTT_X,PTT_Y,PTT_Z,CMNI_X,CMNI_Y,CMNI_Z,CMTT_X,CM_TTY,CM_TTZ,Voxels,Peak_zScore,NS_1,NS_2,NS_3,NS_4,NS_5,NS_6,NS_7,NS_8,NS_9,NS_10
Right Caudate; Right Caudate Body; Right Lentiform Nucleus; Right Putamen; Right Lateral Globus Pallidus,Left  Thalamus; Left  Medial Dorsal Nucleus; Right Thalamus; Right Medial Dorsal Nucleus; Left  Ventral Lateral Nucleus; Left  Ventral Anterior Nucleus; Left  Anterior Nucleus,-2,-11,2,-2,-11,3,17,4,14,17,5,13,2667,5.9317,thalamus; 12.38; 0.83; 0.57; 0.6,midbrain; 8.89; 0.86; 0.32; 0.27,sexual; 8.41; 0.89; 0.12; 0.13,hypothalamus; 6.57; 0.85; 0.16; 0.2,intensity; 5.54; 0.76; 0.08; 0.07,nucleus; 5.32; 0.73; 0.28; 0.36,men women; 5.09; 0.84; 0.01; 0.0,nucleus accumbens; 4.67; 0.78; 0.03; 0.18,accumbens; 4.59; 0.78; 0.02; 0.18,emotion regulation; 4.54; 0.78; 0.01; 0.05
Right Inferior Parietal Lobule; Right Superior Parietal Lobule; Right Brodmann area 40; Right Brodmann area 7; Right Precuneus; Right Angular Gyrus; Right Supramarginal Gyrus,Right Precuneus; Right Angular Gyrus; Right Brodmann area 39; Right Inferior Parietal Lobule; Right Brodmann area 19,28,-61,35,28,-57,35,33,-57,44,32,-53,43,1057,5.1563,intraparietal; 5.92; 0.74; 0.54; 0.5,intraparietal sulcus; 5.85; 0.74; 0.52; 0.48,task; 5.35; 0.66; 0.3; 0.52,parietal; 4.65; 0.64; 0.5; 0.52,letter; 4.34; 0.78; 0.08; 0.09,engagement; 4.24; 0.74; 0.01; 0.02,posterior parietal; 4.23; 0.71; 0.39; 0.34,visual; 4.14; 0.63; 0.49; 0.37,attentional; 4.12; 0.68; 0.29; 0.33,tasks; 4.03; 0.64; 0.3; 0.45
...
```

