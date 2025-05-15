# XMC Reddening Map
3 arcminute resolution reddening map of the XMCs created by combining OGLE-IV (Skowron et al. 2021) and the re-calibrated Schlegel et al. 1998 maps. 

Below is an example code to query the reddening map. You need a minimum of data['ra'] and data['dec']

# Oden E(B-V) reddening maps
from astropy.io import fits
from astropy.wcs import WCS
from astropy.coordinates import SkyCoord
from scipy.ndimage import map_coordinates

exthdu = fits.open('/yourdirectory/oden_ebv_3arcmin.fits')
extim = exthdu[0].data
exthead = exthdu[0].header

wcs = WCS(exthead)  # in galactic coordinates
coo = SkyCoord(data['ra'],data['dec'],unit='degree',frame='icrs')

glon,glat = coo.galactic.l.degree,coo.galactic.b.degree
x,y = wcs.world_to_pixel(coo)

# interpolate values to these points
ebv = map_coordinates(extim, [y, x], order=3, mode='constant', cval=np.nan)
