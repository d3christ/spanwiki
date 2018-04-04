# Publication

## Contents
  1. [Making images for publication with AFNI](#making-images)
      1. [Making colors correspond to p-values](#making-colors)
      2. [Other ways to improve your images – use these if jpeg resolution is bad!](#other-ways)
  2. [Navigation between AFNI, Photoshop, & Powerpoint](#navigation)
      1. [Image preprocessing](#image-preprocessing)
      2. [Figure making](#figure-making)
  3. [SPM Analysis](#spm)
  4. [Writing a paper](#writing)
  5. [Choosing a journal / Impact factor](#choosing)
  
<a name='making-images'></a>
## Making images for publication with AFNI

<a name='making-colors'></a>
### Making colors correspond to p-values
  1. In AFNI, bring up the brain views you would like to image.
  2. Set the color bar to use the desired number of colors
  3. Unset ‘AutoRange,’ manually set the range to 10
  4. Make sure slider goes from min of-10 to max of 10
  5. Use slider to figure out z-score corresponding to p-values; remember we talk about p in power of 10
  6. 1^-4 - 10^-5 = 3.889 (z-score)
  7. 1^-5 - 10^-6 = 4.414 (z-score)
  8. Put the boundaries of the color regions at the corresponding z-score, but remember that the range of the color bar is going to be 1 and divide by 10
  9. Red = .33 (p<.001) and above
  10. Orange = .39 (p<.0001) and above
  11. Yellow = .44 (p<.00001) and above
  12. Same idea for negative values (blue range)
  13. Then drop slider all the way down if nonsignificant range is set to no color, or set slider at threshold (e.g. 3.28 for p<.001, or more leniently at p<.01 for illustrative purposes)
  14. Save images in jpeg format by right-clicking on the image.ppm button at the bottom of every brain view (here you can also save mpeg movies, etc). the default .ppm option is workable but hard to open in most programs.
  
<a name='other-ways'></a>
### Other ways to improve your images – use these if jpeg resolution is bad!
  - Play with contrast (click and hold on image and move mouse up or down) to make backgound black instead of grey – this helps a lot!
  - Resample overlay in cubic mode (‘Define Datamode’ → ‘stat resam mode’ (select ‘cu’)
  - Better yet, in Define Datamode, also click the top right box for ‘warp underlay on demand’ and change the sampling to .1mm. This smooths everything really well, but it makes AFNI run slowly, so make sure you’re in the right plane before capturing. Note that you’ll probably have to photoshop-in the crosshairs with this option because they get really thin.
  - Make sure you know whether you’re in radiological or real-life L-R orientation. AFNI defaults to radiological R=L.
  - Change crosshair colors (white looks nice) and turn off gap, or turn off crosshairs
  - If crosshairs are on, put them in a sensible location (it’s nice to put them at 0S/0A/0R) if in a saggital view but otherwise zero out two axes.
  - You might want to change color specifics, as the default orange is pretty close to red… To do this, just double click on the color you want to change in the range bar.
    - Note: if you can figure out how to get these settings to automatically appear at startup, you win a trip to meet Bob Cox.
    
<a name='navigation'></a>
## Navigation between AFNI, Photoshop, & Powerpoinnt

<a name='image-preprocessing'></a>
### Image preprocessing
  1. Secure copy (scp) the image to a computer with photoshop. Open in photoshop, and change the image to 300dpi and about 5” wide.
  2. Change the contrast to make the background black and the white matter whitish by changing Contrast/Brightness and Levels.
  3. Add in new crosshair lines (if image was resampled at .1mm), using 3-5 pixel line tool
  4. Save as either photoshop PSD or tiff (collapse layers before saving to tiff)
  
<a name='figure-making'></a>
### Figure making
  - Still in Photoshop, if using multiple brain images (like a group comparison), import the color bar from insula:Desktop/SPANlabdocs/usefulimages folder
    - (you may need to open the color bar in Powerpoint, copy all elements, and paste into a new Photoshop file). Place images on black background.
  - BK likes to use Powerpoint to compose figures. This step isn’t necessary, however. The tiff created in Photoshop can be used as the final product.
  - If a tiff needs to be generated from a Powerpoint file, THE BEST or ONLY way of making a decent-looking file is to simply select all the ppt elements, copy. Then, in Photoshop, control-n / apple-n to open a new file, and simply PASTE in all the copied Powerpoint elements. Don’t ask why this is how to do it. Or ask Bill Gates. MANY hours have been wasted on this task, so remember this solution and stop the insanity.
  - We don’t have Adobe Illustrator, but have been meaning to get it for a while because it might be cool.
  
<a name='spm'></a>
## SPM Analysis
ASK JEFF. If Jeff is gone, you are probably out of luck. BUT if you’d like to run an SPM-like analysis (e.g. for a bar graph of beta coefficients for single conditions like $5 gain trials) ask Greg because there is an option in 3dDeconvolve for such atrocities.

<a name='writing'></a>
## Writing a paper
[Brian mentioned he’d write up a blurb for this.]()

<a name='choosing'></a>
## Choosing a journal / Impact factor
[BRIAN’s notes from 041607 lab meeting]()
