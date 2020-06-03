# This folder is for configuration files of SExtractor+PSFex for USNAC.

## 1st step: Run SExtractor to dectect PSF candidates.
`usnac_sex_1st_run.(config / params)`, based on ZOGY default and `ztfSexConfigAstrom.inp` of Franck.

## 2nd step: Run PSFEx.
`usnac_psfex.config`, based on ZOGY default.

## 3rd step: Run SExtractor for PSF photometry.
`usnac_sex_2nd_run.(config / params)`, based on ZOGY default and `ztfSexConfigFinalRun.inp` of Franck.
