# Warping VOI Masks

For information on the mask type and mask creation, see [Making VOI masks](making-voi-masks.md).

After making a mask using the draw dataset plugin, the mask is typically in +tlrc space and is usually sampled at 1mm. In order to accurately dump timecourse data from these regions, the mask must be warped and resampled. For the sake of storage contraints and accuracy, the adwarp command should no longer be used. Adwarp typically would involve a non-optimal, additional manipulation step on the functional dataset. Instead, 3dfractionize and 3dresample should be used only on the mask file to down-warp the mask to +orig and resample to match the functional dataset space (usually 3.75mm 3.75mm 4.00mm).

__Canonical VOI Masks and Individual VOI Masks (in tlrc space):__

```
$ 3dfractionize -template [subject's functional dataset +orig] -input [mask file +tlrc] -warp [subject's sag116+tlrc] 
  -clip 0.1 -preserve -prefix [output, append 'r' to input mask name +orig]
e.g.
$ 3dfractionize -template zmidantoutreg+orig -input apVOImask+tlrc -warp sag116+tlrc -clip 0.1 -preserve -prefix    
  apVOImaskr+orig
```
Notes:
  - Make sure you use subject specific files for warp and template. The template should be functional dataset you will eventually dump data from.
  - Use exact coordinate spaces specified above (+tlrc/+orig)
  - Clip should not be changed unless you notice that too much or not enough of the starting mask size is removed. Increase to clip additional voxels, decrease to restore.
  - When the command is executed you should end with a maskfile that is resampled to the functional dataset coordinate space and down-warped to +orig space. You can verify this by using the 3dinfo command in terminal on the output mask file.
  
__Masks made in Original Space:__

In some cases your mask may be made in original space (some individual masks). Warping is unnecessary since the mask is already in +orig space. However, they do need to be resampled to match the functional dataset that will be used to dump from. It is very important that you use the correct dimensions in the -dxyz option. Running 3dinfo on the functional dataset will provide the correct dimensions (usually, 3.75 3.75 4.0). Output will be a mask file in +orig space that has voxels that are the same dimensions as the functional dataset.

```
$ 3dresample -orient LPI -dxyz DX DY DZ -prefix [output, append 'r' to input mask name] -inset [input mask +orig]
e.g.
$ 3dresample -orient LPI -dxyz 3.75 3.75 4.00 -prefix apVOImaskr -inset apVOImask+orig
```
