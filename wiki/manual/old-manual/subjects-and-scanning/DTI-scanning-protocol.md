# DTI Scanning Protocol

## Contents
  1. [DTI Scanning Protocol Introduction](#introduction)
  2. [Protocol Instructions](#instructions)
  3. [Creating a New DTI Protocol](#new-protocol)

<a name='introduction'></a>
## DTI Scanning Protocol Introduction
Running a DTI Scan (Diffusion Tensor Imaging) is pretty easy, but there are a few more steps to set up your DTI prescription.

### Multiple Iterations
You get nicer data if you run more iterations of a DTI scan. The more iterations you can run, the better your data is. A single DTI scan through the whole brain on the 1.5T using the protocol for the FINRA project takes around 11 minutes. Two scans takes 22 minutes, Three scans takes 33 minutes, etc. After you set up the first scan, doing another is easy. Just hit scan again.

Each 11m scan collects 5,220 dicoms. Yes, that's five-thousand two-hundred twenty.

<a name='instructions'></a>
## Protocol Instructions
You can not prescribe your DTI until after you have collected a hi-res T1 (3d volume)

The only thing new about doing a DTI is setting up the prescription:
  1. Choose your DTI-40dir Series from the Rx Manager menu in the lower left. Double click, or click on View Edit.
  2. Click on Graphic Rx
  3. Click Select Series
  4. Choose the 3d volume and click on Apply All (this will put sagittals on all three viewing windows)
  5. Click somewhere on the middle sagittal slice of the brain - this will place a huge array of axial slices on the image
  6. Use the handles, the little circles, to change the horizontal axis of the DTI prescription. The goal is to get the lines to be parallel to the longest imaginary line that you can draw across the corpus callosum. Imagine a line from the most anterior end of the corpus callosum to the most posterior end of the corpus callosum. You want your DTI slices to be parallel to those.
  7. Drag the prescription around to cover the whole brain.
  8. Save Series > Download > Scan
  9. If you are doing multiple scans, after the first DTI, hit Scan again.

<a name='new-protocol'></a>
## Creating a New DTI Protocol
Ask Greg Samanez Larkin or Bob Dougherty (from the Wandell lab).

__IMPORTANT:__ To use most DTI prescriptions at the Lucas Center, you must first get authorization from Roland Bammer. He is usually very responsive to email.
