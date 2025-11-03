# 2.3.3 - 09/03/24
	- Restored DetailTex functionality on volumetric clouds for modders

# 2.3.2 - 28/02/24

- Fixes:
	- Fix missing ocean and other effects when applying any of the in-game video settings in flight or when changing game resolution or monitor
	- Fix occasional nullref spam caused by ocean failing to load when switching ships from tracking station or loading a savegame
	- Fix black sky issue on intel integrated graphics and Macs
	- Fix white sky issue on slow GPUs when generating atmos

# 2.3.1 - 30/01/24

## New features:
	- Make it possible to combine SMAA and TAA for better AA and the best of both worlds
	- Make it possible to disable TAA below a configurable fps threshold (26  by default) and fallback to SMAA-only if TAA and SMAA are both enabled
	- Support for new non-Kerbin homeworld name (see latest versions of Kopernicus)

## Fixes:
	- Fixed V4 clouds not working correctly on Mac and appearing blocky and undefined (ie with no noises applied)
	- Fixed several issues leading to a black sky on all platforms, a few cases remain which I'm investigating
	- Fixed error leading to a double image with an old image of the craft stuck on screen, notably when switching to Jool from tracking station
	- Fixed saturated planets in scaled space being caused by the scaled color adjustment feature in Scatterer
	- Fixed stock ocean occasionally reappearing and conflicting with scatterer ocean
	- Fixed black scaled scattering when switching to bodies from tracking station
	- Fixed atmo failing to load if EVE integration is disabled in the main settings

# 2.3.0 - 22/12/23

## New features:
	- New lighting model: Better handling of multiple scattering and better interplay of direct lighting and ambient, clouds look fluffier.
	- Light volume: Long-distance cloud shadows and ambient handled via separate system. Updates in the background, has its own settings.
	- Physically accurate godrays: Scatterer feature, clouds can now cast godrays using the above light volume, with better physical accuracy than previous methods. Can also be used for terrain shadows, see settings in the KSC screen Scatterer menu.
	- Screenshot mode: Disable TAA and other temporal effects when taking screenshots. Make screenshots compatible with supersizing and free of temporal artifacts/blur
	- Native cubemap support: Cubemaps are now loaded as a native hardware cubemap instead of being sampled as 6 separate textures, improves performance on configs using cubemaps, notably RSS.
	- Additional temporal upscaling settings including 3x, 6x, 9x and 12x for better quality/performance tuning.
	- Exposed phase functions for single and multiple scattering for clouds
	- Added signed distance fields to skip empty space when raymarching, limited performance gains, see wiki https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Signed-Distance-Fields
	- Added scatterer option to use only SMAA in IVA when using TAA, to limit blurring on droplets and other elements because some cockpits have glass that is treated as opaque.
	- Added scatterer setting to reduce the extinction tint on sunlight at noon
	- Added option to unload clouds painters (to free up memory or load new textures)
	
## Configs:
	- Tweaked Kerbin's atmosphere to make sunsets/sunrises look better with the new godrays
	- Tweaked all cloud configs to look better with the new lighting (go see the new settings on Eve)
	- Separated Scatterer configs into separate folder "StockScattererConfigs"

## Fixes
	- Fixed distracting black artifacts on the clouds that would appear around the horizon or in movement
	- Fixed blue edges/outline which appear when a cloud layer is seen through an other, typically when looking at clouds through fog or rain
	- Fixed visible "barrier" between fog/rain and the background
	- Fixed cloud upwards noise animating way faster than it should in timewarp, causing artifacts
	- Fixed ozone causing a strange tint to appear on crafts in orbit with rescale