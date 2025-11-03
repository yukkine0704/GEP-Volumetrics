# 2.1.3 - 11/06/23
- Fixed OpenGL (Mac and Linux) issue where Jool clouds are cut off before the horizon from high altitude
- Improved scatterer wave interactions to make boats and sea planes easier without overriding the stock water drag (no longer affecting the max speed of submarines), only removed the excessive drag when hitting waves

# 2.1.2 - 07/06/23
- Fixed engine/rocket sounds getting disabled when there is a lot of thunder
- Fixed clouds disabling in map view when going under the surface of Jool
- Fixed Jool flowmap not being paintable because of duplicated 2d layers

# 2.1.1 - 28/05/23
- Fixed Kerbal IVA flashing repeatedly on screen in some cases
- Fixed "Invalid scenehandler" log spam
- Fixed repetitive crashing in some cases
- Fixed terrain tiles detaching and creating holes in some cases

# 2.1.0 - 21/05/23

## New features:
	- Particle effects, used for actual rain (with splashes on surfaces), snow and dust + sound effects
	- Lightning strikes, with sound effects, light emission and dedicated bolt textures
	- Flowmaps to animate both the scaled texture and the volumetrics, used on Jool for swirls and moving bands

## Configs:
	- Jool config: moving clouds, swirls and bands. Best enjoyed in timewarp. Ability to go much further down inside Jool than the previous -250m limit. Higher quality cloud settings than on the other planets.
	- Kerbin: Added lots of anvil clouds, rain and lightning placed under/around anvils. Decreased frequency of fog and rain and fixed fog happening around the planet all at once. The top cloud layer is now wispier.
	- Duna: made the dust storms less noisy and made them darken the ground. Added a few rotating dust devils.
	- Laythe: Plumes now move in the direction of the wind not just upwards.

## Fixes:
	- Fixed hard edges on the rain on Kerbin
	- Fixed eclipses on volumetrics and scattering, not just the sky. Eclipses on Laythe are now scary
	- Fixed harsh line/cutoff on the terminator on the clouds of Kerbin
	- Fixed OpenGL/mac/linux support
	- Fixed alphamap support for the in-game painter
	- Fixed some issues with scatterer sunColor not applying to volumetrics
	- Fixed error messages with the screen not being divisible by 4 * 2 and clouds not loading
	
## Other:
	- TimeSettings now have a unit setting and can be set in hours instead of seconds, defaults to seconds for retrocompatiblity.
	- RaymarchedVolume color settings now accepts both 255 RGB colors and 1-0 float colors
	- Added brush hardness setting in the painter