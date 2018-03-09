# Dumping Raw Timecourse Data

Raw timecourse data does not undergo any averaging or motion correction (aside from preprocessing). Output from a raw timecourse dump will consist of a .tc file with a single column. A row value will be included for every TR. This type of file is useful if you would like to pluck out individual trials.

## Dumping out the data with 3dmaskave

To dump out raw data, you will NOT be using the iresp method. The functional file you will use will be output from a typical preprocessing script (ex. high pass filtered, normalized \[converted to percent signal change]). Before continuing with this step, you must have created a mask within afni and resampled/warped the mask so that it matches your functional dataset. Instructions to do so are found [here](making-voi-masks.md).

3dmaskave takes your mask and the dataset, created after preprocessing, and dumps out the data only for the voxels designated by your maskfile. The values for these voxels are then averaged across the VOI.
```
3dmaskave -mask [mask, resampled and warped to +orig] \ 
-quiet -mrange 1 1 -drange [maskrangehi] [maskrangelo] [functionaldataset+orig] > [outputname.rawtc]
e.g.
3dmaskave -mask amVOImaskr+orig \
-quiet -mrange 1 1 -drange 210 -190 IncDectcs+orig > IncDectcs_NAcc.rawtc
```
Notes:
  - If you're dumping out timecourses after highpass filtering (using 3dFourier), you may notice that your first two TRs have the same activations. This, however, is not a timing issue, just a byproduct of the Fourier transform.
  - Your mask file usually will have multiple VOIs. You must specify which mask you would like to dump from. This is done in the -mrange option. Mrange takes two values (a, b) that tell the function to dump only the voxels that have values between a and b (inclusive). You can specify a single VOI by repeating it's index number.
  - Using the mask option you will specify the +orig, resampled mask. Voxel dimensions and coordinate space must match the file you are dumping from.
  - The output is a '.rawtc' file which is a column of values and can be viewed with a simple text editor.
  - The drange option allows you to specify a range, relative to baseline, to limit the percent signal change. The values impose a maximum percent signal change of %10. Motion correction in preprocessing will catch most of the abnormally high changes in signal but this is an additional safeguard. The values above may be used study to study be should be re-evaluated if the baseline changes (ex. using 3T data).
  - Always be sure the output name includes the correct VOI name.

--Code examples taken from the FINRA MIL Study.
