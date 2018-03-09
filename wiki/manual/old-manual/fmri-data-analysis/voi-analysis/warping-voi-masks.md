# Warping VOI Masks

For information on the mask type and mask creation, see [Making VOI masks](making-voi-masks.md).

After making a mask using the draw dataset plugin, the mask is typically in +tlrc space and is usually sampled at 1mm. In order to accurately dump timecourse data from these regions, the mask must be warped and resampled. For the sake of storage contraints and accuracy, the adwarp command should no longer be used. Adwarp typically would involve a non-optimal, additional manipulation step on the functional dataset. Instead, 3dfractionize and 3dresample should be used only on the mask file to down-warp the mask to +orig and resample to match the functional dataset space (usually 3.75mm 3.75mm 4.00mm).
