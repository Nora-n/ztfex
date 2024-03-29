# ZTFEx
Collaborative repository for tracking advance on a personnal implementation of
SExtractor+PSFEx for ZTF data

## Required files

The `.fits` files are to be stored locally in the
[Data](../../tree/master/Data) folder. We are currently working with the
following files:
- `mskimg`
- `psfcat`
- `sciimg`
- `sciimgdaopsfcent`

that can be taken from
[here](https://irsa.ipac.caltech.edu/ibe/data/ztf/products/sci/2019/0124/095417/)

The `sciunc.fits` file has to be created from the `sciimg` one. One can do so
with the following:
```python
from astropy.io import fits
from ztfimg import image

root = '../Data/'
sciimg = root + 'ztf_20190124095417_000403_zr_c01_o_q1_sciimg.fits'

hdul = fits.open(sciimg)
header = hdul[0].header

z = image.ScienceImage(sciimg, mask)
z.load_source_background()

file_out = sciimg[:-11] + 'sciunc.fits'
sciunc_out = fits.HDUList(fits.PrimaryHDU(data=z.sourcebackground.rms(),
                                          header=header))
sciunc_out.writeto(file_out, overwrite=True)
```
as can be found in the [psf_comp](../../tree/master/Notebooks/psf_comp.ipynb) notebook.

## Running SExtractor
We use the following command to obtain the `test.cat` output of SExtractor, which will be inside [BlackGEM_default](../../tree/master/Data/config_ZOGY/BlackGEM_default/) as defined in the first line:

```
sex ztf_20190124095417_000403_zr_c01_o_q1_sciimg.fits -c config_ZOGY/BlackGEM_default/sex.config -SATUR_LEVEL 50000 -SEEING_FWHM 2.58933692741394 -PIXEL_SCALE 1.012 -MAG_ZEROPOINT 0 -WEIGHT_IMAGE ztf_20190124095417_000403_zr_c01_o_q1_sciunc.fits,ztf_20190124095417_000403_zr_c01_o_q1_sciunc.fits -FLAG_IMAGE ztf_20190124095417_000403_zr_c01_o_q1_mskimg.fits -CATALOG_TYPE FITS_LDAC -CHECKIMAGE_TYPE OBJECTS -CHECKIMAGE_NAME ztf_20190124095417_000403_zr_c01_o_q1_sexcat_ldac.fits -VERBOSE_TYPE NORMAL
```

If you wish to use another configuration file, simply change `BlackGEM_default` to another folder in [config_ZOGY](../../tree/master/config_ZOGY). If you wish to change where the output goes, change the `sex.config` and `psfex.config` files of the subdirectory you chose accordingly.

## Running PSFex
We use the following command to obtain the `test.psf` and `psfex_out.cat` outputs of PSFEx, which will be inside [BlackGEM_default](../../tree/master/Data/config_ZOGY/BlackGEM_default/) as defined in `psfex.config`:
```
psfex config_ZOGY/BlackGEM_default/test.cat -c config_ZOGY/BlackGEM_default/psfex.config
```

## Doing photomotry
See [psfex-ztflc_fit](../../tree/master/Notebooks/psfex-ztflc_fit.ipynb) notebook.
