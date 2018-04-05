# AFNI

## Contents
  1. [polort rule of thumb](#polort)
  2. [3dRegAna](#3dregana)
      1. [-model tag](#model-tag)
      
<a name='polort'></a>
## polort rule of thumb
AFNI folks suggest that for every 150 seconds you should add a degree to the polynomial.

For a 150 second long scan, you would use -polort 1, for a 300 second scan, you would use -polort 2, and so forth.

<a name='3dregana'></a>
## 3dRegAna
3dRegAna is an AFNI program which can be used to do multiple linear analysis

<a name='model-tag'></a>
### -model tag
The -model tag can be used to specifiy and test regression models
```
 -model 1 : 0
```
will test Yi = B0 + B1X1i+ei vs. Yi = B0 + ei
```
 -model 2 : 0
```
will test Yi = B0 + B2X2i+ei vs. Yi = B0 + ei
```
 -model 2 : 1 0
```
will test Yi = B0 + B1X1i + B2X2i+ei vs. Yi = B0+ B1X1i + ei
```
 -model 2 1 : 0
```
will test Yi = B0 + B1X1i + B2X2i+ei vs. Yi = B0+ei
