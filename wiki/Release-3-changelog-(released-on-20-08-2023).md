# 2.2.1 - 26/08/23
- Fixed textures not loading when using steam launch options
- Fixed low fps in main menu

# 2.2.0 - 20/08/23

## New features:
	- Cockpit Droplets: Droplets and streaks will now be seen in IVA view moving along windows and windshields when raining or flying through a dense cloud, they refract the background and evaporate at supersonic speeds. Their motion matches the craft speed and movement direction
	- Separate sound file can be played in IVA (used here to add noise of rain hitting winshield)
	- Load on demand (reduce RAM and VRAM usage, faster load times if not using community fixes)
	- BC4 texture support
	- Higher quality cloud shadows closely matching volumetrics (when volumetrics are enabled)
	- New orbit mode for temporal upscaling, improving image quality and stability from orbit and long distances, can be disabled if needed
	- Ozone layer support for Scatterer
	
## Configs:
	- Rain on Kerbin is now more frequent and it's now always raining somewhere on the planet
	- Fixed some seams on Kerbin cloud textures
	- Kerbin's atmo tweaked to be more earth-like
	- Added falling snow on Laythe
	- Diffused cloud cover on Laythe poles, to allow for occasional views of Jool from the poles, previously cloud cover was constant

## Fixes
	### - Clouds:
		- Improved temporal upscaling to work better in motion and from orbit, should have less artifacts and blurring
		- Fixed high temporal upscaling settings resulting in weird line artifacts on clouds
		- Fix artifacts on cloud edges intersecting terrain/craft
		- Fix issue on cloud self-shadowing forming a stepping artifact near tops of cloud layers
		- Fix cubemap support for flowmaps
		- Clouds now render in HDR buffers if HDR is enabled (less banding if you have a HDR monitor)
		- Fix teapplying a volumetrics layer in map view breaking PQS terrain
		
	### - Scatterer:
		- Fixed HDR support in Scatterer (both with and without TAA), sun can now be seen gliting off reflective crafts with TU (see images on patreon post) if using HDR and bloom from TUFX.
		- Fix mie scattering ending at a visible line before the terminator when using thick mie scattering
		- Fix sunflare being offset from the sun during high timewarp
		- Improvements to Scatterer TAA: Less ghosting and sharper TAA algorithm
		- Fix sky not reflecting off crafts in OpenGL
	
