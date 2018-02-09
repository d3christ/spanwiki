You'll often find yourself wanting to create images out of a whole brain map. 

To help with this, Nick wrote the `spantoolbox/images/@afni_image` utility. You use it from the command line like so:

```

Usage: @afni_image <-underlay ANAT -overlay -DSET>

 Return a jpeg image of DSET overlayed  on ANAT.

   By default, create a montage. Use dxyz with -no-montage
       if you want a single location. Works best if ANAT/DSET in cwd

   Example: @afni_image -u anat.nii -o zstock_reg_csfwm_masked+orig \
                -prefix reg -sub 3
        Creates a 9x4:3 montage of subbrick 3 overlayed on top of anat
        Returns montage for each plane; contains most of the brain.

   Example: @afni_image -u TT_N27.nii -o nacc8mm+tlrc -dxyz 10 -12 -2 \
                -prefix nacc_image -no-montage -threshold 0
        Creates 3 (cor/sag/axi) images showing left side of a nacc
        ROI at coordinates  10, -12. -2 on the Colin brain.

   ========================= OPTIONS ===============================

   -no-montage       Use this option to turn montage off.
   -prefix:          The name that will be given to the jpeg outfile.
                     Unless absolute path is given, prefix path is
                     to be in the same directory as -underlay
   -sub:             Tell AFNI to print this subbrick [Default: 0]
   -dxyz x y z:      Where to center the images. [Default = 0 0 0]
   -threshold:       What to set the threshold [Default 2575=p<0.01)].
                     Use 1960 for p<0.05. No cluster correction;
                     to correct your images, use 3dClustSim first.
   -montage_size:    How to lay out the montage. [Default 9x4:3]. Last
                     number is the number of slices  between frames.
   -labels:          Display coordinates over montage frames.

```

If you find it helpful, consider putting it into your /usr/bin folder so that it will work in any directory by just saying `@afni_image`. Otherwise you'll need to type `/path/to/@afni_image`.


# Using it with common ROIS:

Check out the example below of a script that takes a number of ttest results and prints images at some of our common ROIs, which can be useful in certain situations:

```
#!/bin/csh
# For a normal set of vois and a set of whole_brain images, render to image.

set underlay = TT_N27.nii

foreach overlay ( choice_z feedback_z inflection_z )

    # Left Nacc

    set outfile = nacc8mml_${overlay}

    @afni_image -u TT_N27.nii -o ${overlay}+tlrc -dxyz 10 -12 -2 \
                -prefix $outfile -no-montage

    # Right Nacc
    set outfile = nacc8mmr_${overlay}

    @afni_image -u TT_N27.nii -o ${overlay}+tlrc -dxyz -10 -12 -2 \
                -prefix $outfile -no-montage

    # Left mpfc

    set outfile = mpfcl_${overlay}

    @afni_image -u TT_N27.nii -o ${overlay}+tlrc -dxyz 4 -47 0 \
                -prefix $outfile -no-montage

    # Right mpfc
    set outfile = mpfcr_${overlay}

    @afni_image -u TT_N27.nii -o ${overlay}+tlrc -dxyz -4 -47 0 \
                -prefix $outfile -no-montage

    # Left Insula - Desai

    set outfile = desai_insl_${overlay}

    @afni_image -u TT_N27.nii -o ${overlay}+tlrc -dxyz 32 -30 -2 \
                -prefix $outfile -no-montage

    # Right Insula - Desai
    set outfile = desai_insr_${overlay}

    @afni_image -u TT_N27.nii -o ${overlay}+tlrc -dxyz -32 -30 -2 \
                -prefix $outfile -no-montage
end

```

If you wanted to make a small montage at each of these, you might consider changing the lines to something more like 
```
@afni_image -u TT_N27.nii -o ${overlay}+tlrc -dxyz -32 -30 -2 \
                -prefix $outfile -montage_size 3x3:1
```
Which will make a 3x3 montage at each location in each plane. 
