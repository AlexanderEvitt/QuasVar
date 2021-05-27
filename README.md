# QuasVar
>Code for a quasar variability study in collaboration with @esellers04

This code is designed to complete a quasar variability study using Transiting Exoplanet Survey Satellite (TESS) data and Long Short-Term Memory Recurrent Neural Networks (LSTM-RNN).

To operate effectively, clone to a repository and set up a a second directory where data can be stored. Run **quasar_catalogue.csv** with data from sources below to obtain the catalogue and its contents or email alexander.evitt@gmail.com for a copy of the data.

Catalogue of quasars taken from https://www.sdss.org/dr12/algorithms/boss-dr12-quasar-catalog/
Mass data taken from https://arxiv.org/pdf/1609.09489.pdf, data headers are shown below

Variable | Info
------------ | -------------
RAdeg | Right Ascension in decimal degrees
DEdeg | Declination in decimal degrees
z | Redshift
Mi | Absolute Magnitude i-band 
L5100 | log10(L at 5100A in erg s^-1)
eL5100 | error in log10(L at 5100A in erg s^-1)
L3000 | log10(L at 3000A in erg s^-1)
eL3000 | error in log10(L at 3000A in erg s^-1)
L1350 | log10(L at 1350A in erg s^-1)
eL1350 | error in log10(L at 1350A in erg s^-1)
MBH_MgII | log10(M_BH in units on M_SUN from MgII)
MBH_CIV | log10(M_BH in units on M_SUN from CIV)  
Lbol | log10(bolometric L in erg s^-1)
eLbol | error in log10(bolometric L in erg s^-1)
nEdd | log10(Eddington ratio)

**locator.py** is modified from https://github.com/kristalynne/tess_agn


Below is some python-ish pseudocode that shows how we generate lightcurves
```python
for row in target_list:
    try:

        star = eleanor.multi_sectors(sectors='all',tic=0,coords=(ra,dec),tc=True)

        full_lc = lightkurve.LightCurve([],[])
        for observation in star:
            data = eleanor.TargetData(observation,bkg_size=15,do_pca=False)
            data.custom_aperture(shape='rectangle', h=1,w=1, method='exact')
            data.get_lightcurve()

            q = data.quality == 0

            lc = data.to_lightkurve()
            lc.flux = lc.flux - numpy.median(lc.flux)
            full_lc = full_lc.append(lc)

            save_lightcurve(sector_lightcurve_path,sector_lightcurve)
        save_lightcurve(full_lightcurve_path,full_lc)
    except:
        terrible_targets.append(row)
        continue
```




test
