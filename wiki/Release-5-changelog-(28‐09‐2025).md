******* 30/09/25 *******

- Fix Scatterer causing parts in the VAB to become dark

- Deferred updates (download separately)
	- Fix Deferred SSR getting "phantom" bright white reflections on Minmus when there is nothing to reflect
	- Force reflection probes on to fix some users getting no ambient lighting because they disabled it
	- Check for NaNs when doing lighting and SSR to attempt to fix some issues with SSR becoming black


******* 3.1.0 - 29/09/25 *******

- Fix serious issue causing kraken attack (ships going off-course or disintegrating) when switching to map view in some conditions

******* 3.0.0 - 28/09/25 *******

- New features:

	- Improved volumetric clouds
		- Reworked noise and shading to get more realistic-loking clouds
			- Improve noise quality, more detail can now be extracted out of a single baseNoise texture
			- More control over noise is provided through persistence and lacunarity variables and how "deep" inside the cloud erosion is performed. Also added a setting to get a more billowy/rounded noise
			- Remove obsolete noise/brightness settings that are difficult to control and direct
			- Remove obsolete detail noise
		- Improved cloud upscaling
			- Reduced noise
			- Fixed noise appearing on sudden camera movements
			- Add a distance-based continuous accumulation setting, this increases noise absorption at long distances allowing larger step sizes. Distance adjustment is a trade-off for smearing (which falls off with parallax/distance)
		- Add density gradient curves to make clouds get denser over height
		- Upgrade anti-tiling method, less 3d noise tiling will be noticeable on clouds
		- Old configs are incompatible, change node name to layerRaymarchedVolumeV5 to not load old configs, bump major version
		- Handle overlapping volumetric layers, the solution is not perfect and overlapping layers should ideally be rendered densest to thinnest, added a manual "overlapRenderOrder" setting to control this
		- Add/fix normalmap support on 2d clouds
		- Add a lambertian setting to 2d clouds to finetune the shading, previous versions (With scatterer integration) would be the equivalent of lambertian set to zero
		- Add loading of BC5 format for normalMaps in EVE (with on-demand texture loading)
	
	- Wet surfaces
		- Add puddles that accumulate when it rains and slowly dry off, accumulation is simulated locally and can happen off-screen and on game/planet loads
		- Add different settings to modify Buildings/Terrain/Craft' smoothness and albedo with wetness and puddle accumulation
		- Add rain ripples on puddles when it is raining
	
	- Add screen-space reflections (using Deferred)
		- Compatible with all deferred shaders and Scatterer Ocean
	
	- Improved lighting and shadows
		- Render clouds in reflection probes making them appear in off-screen reflections and impact ambient light
		- Make scattering render correctly on reflection probes
		- Integrate cloud shadows into the lighting correctly instead of being overlaid on the final image, render cloud shadows at lower resolution
		- Add sunset colors and cloud shadows to the IVA lighting and reflection probes
		- Sort atmospheres and cloud layers back-to-front correctly, most noticeable when observing Jool behind Laythe
		- ParticleFields receive ambient light from reflection probe and shadow from lightVolume
		- Improved accuracy of screen space cloud shadows to the actual volumetrics
	
	- Ocean improvements
		- Add SSR support to ocean
		- Render Scatterer ocean in a Deferred manner, removes the pixel lights cost for the ocean and enables them by default
		- Improve ocean refractions to look more realistic/accurate, using an SSR-like method
		- Additional settings for ocean reflection and ambient colors, for emulating metallic or exotic oceans
		
	- Improved authoring tools
		- Add tile-based painting tools to volumetrics painter
		- Add live preview to painter brush (no longer need to click/paint to see the result, just mouse over)
		- Screen space cloud shadows now see painter modifications
		- Add exporting of 2d cloud maps (albedo, transparency and normals) from volumetric clouds

	- Other
		- Small performance improvement for lightVolume updates
		- Remove obsolete KeepUntilingOnNoFlowAreas option (due to sample count difference between anti-tiling and flow) and add a per-layer option to disable anti-tiling if needed
		- Improve performance on a cloud layer if all cloud types use the same noise scale
		- Add per-type multiple scattering brightness setting, to tweak excessive brightness on very thin clouds
		- Add per-type curl noise strength
		- Add fxOnly option to avoid raymarching layers used only for particles and sounds
		
- Configs:
	- New Kerbin configs
		- Hand-painted cloud formations taking advantage of new cloud features
		- Added several new cloud types better approaching the look of real convective clouds
		- Cumulonimbus clouds are now on their own layer overlapping the main layer for a more complex realistic look
		- Added several tiles to paint realistic cloud formations using the new tile painter
	- Redone Laythe Volcanoes to look more imposing and realistic, taking pyrocumulus clouds as inspiration

- Fixes
	- Fix clouds disappearing below a certain density, making thin clouds not work well
	- Fix pitch black darkness on dark side of planets caused by Eclipse bug in EVE
	- VR fixes by @JonnyOThan
		- Fix SMAA in VR
		- Use the correct resolution in VR for multiple effects
		- Reduce smearing on right eye for volumetric clouds
	- Fade out wave interactions for craft going fast, makes seaplanes easier
	- Fix Volumetric cloud shadows on the ground sliding around with zoom at some distances
	- Make TAA see volumetric clouds motion vectors for less blur on clouds when TAA is active
	- Fix scaled space camera skybox motion vectors adding additional TAA blur
	- Fix artifacts/clipped neighbours on some snow particle texture
	- Fix minor issue with the kerbal IVA portrait viewport matching the main camera's
	- Reduce global keywords and use local keywords to avoid crashes with many mods adding keywords
	- Fix memory leaks when generating atmos or just pressing generate and loading atmos from cache
