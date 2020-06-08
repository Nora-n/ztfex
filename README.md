# ZTFEx
Collaborative repository for tracking advance on a personnal implementation of
SExtractor+PSFEx for ZTF data

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
as can be found in the [Data](../../tree/master/Notebooks) folder.
