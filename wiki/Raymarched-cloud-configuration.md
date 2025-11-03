This section describes how to configure the raymarched clouds. All configurations can be made in-game.

To add a volumetric layer add a layerRaymarchedVolume node to your clouds Object


# LayerRaymarchedVolume settings

* Color: Color of the volumetric layer, will be multiplied by the value of the color map if used.

* SkylightMultiplier: How much light the clouds receive from the (Scatterer) sky. This wasn't always working correctly in Release 1 and sometimes requiring extreme values to look right. This was fixed in Release 2 so older configs may need to be adjusted.

* SkyLightTint (added in Release 2): The tint of the skylight, I tend to use 0 for no tint, because with the lack of white balance in KSP this can overpower all the other colors. Release 1 had a hardcoded 0 tint value so older configs are compatible by default.

* ReceiveShadowsFromLayer (removed in Release 4): Name of a layer on the same planet to receive shadows from, needs to be a layer with raymarched volumetrics, the coverage map of the volumetrics will be used. See [inter-layer shadows](https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Raymarched-Clouds-Overview#inter-layer-shadows), made obsolete with light volume in V4

* ReceivedShadowsDensity (removed in Release 4): Density of the received shadows. Adjust to taste, made obsolete with light volume in V4

* ScaledFadeStartAltitude and ScaledFadeEndAltitude: Altitudes at which the volumetrics start and end fading into the scaled layer. Needs to be the same for all layers on the same planet (otherwise the min value is used anyway).

* UpwardsCloudSpeed: Speed in m/s at which the noise will be scrolled through the volume upwards. Can have negative values for noise going downwards. Use negative values for rain for example and positive values for clouds.

* DetailNoiseTiling: The size at which the detail noise will be tiled, see the noise section below for explanation.

* UseDetailTex: If the detail tex should be used with the coverageMap (useful to create dynamic clouds). This has a performance cost so isn't enabled by default. (Removed in Release 4 initially but restored on 09/03/24 for use by modders)

## Noise

3d noise is used to add detail to the cloud and can be configured here. A small 3d noise texture is generated via these settings.

It will be sampled once per cloud type, with size configurable per cloud type.

Releases before 5 had an additional sample (called detailNoise), with size configurable globally, but strength configured per cloud type. This is no longer needed in release 5 as it gets a lot more detail out of the base noise.

Settings:

You can add a worley node for worley noise, perlin node for perlin noise, or both for a mix (perlin-worley).

Each node will have these settings:

* Octaves: The number of times noise is layered, each octave adds a higher-frequency, lower-amplitude detail layer. in most cases just set this as high as possible.
* Periods: How many periods of the largest octave will be in the noise texture. In previous releases this was a way to have more variation inside the texture, at the cost of "spreading" the resolution over more octaves. As of V5 the anti-tiling does a much better job of adding variation, so just set this to 1 in V5 to have the highest resolution and detail possible in the 3d noise texture.
* Lacunarity (Added in release 5): Controls how frequency increases across octaves. Each octave’s frequency is multiplied by lacunarity, so frequencyN = baseFrequency * lacunarity^n. Only integer values are supported. Lacunarity should be balanced with Persistence for best effect, you'll find some illustrated examples below.
* Persistence (Added in release 5): Controls how amplitude decays across octaves. Each octave’s amplitude is multiplied by persistence, so amplitudeN = baseAmplitude * persistence^n. Typical values range from about 0.25 to 0.75; higher persistence preserves more high-frequency energy and gives rougher, more contrasty detail, while lower persistence rapidly attenuates finer octaves and produces smoother results. For best effect balance it with Lacunarity.

With lacunarity and persistence it is important not to push the "detail" too high so that the individual octaves do not separate too much, otherwise the cloud stops feeling like a cohesive whole and starts to feel like dissolving particles. If in doubt use the default settings from Kerbin's config.

![alt](https://images2.imgbox.com/bc/84/1f20kVUT_o.png)
Example with lacunarity 2.0, from left to right persistence 0.4, 0.6 and 0.7

![alt](https://images2.imgbox.com/1e/c4/rwGL3S1C_o.png)
Example with lacunarity 3.0, from left to right persistence 0.4, 0.5 and 0.6, note how higher persistence values "greeble" the cloud much faster with higher lacunarity

* Erosion depth (Added in release 5): How far inside the cloud the noise-based erosion will apply. Imagine the cloud body as a gradient from outside to inside, this decides how far towards the inside of the cloud chunks will be carved out with noise. It is important to find a good balance here, not deep enough and the cloud looks blobby and not detailed, too deep and the cloud starts to dissolve and lose it's structure.

![alt](https://images2.imgbox.com/c4/1f/DlMGUNWb_o.png)

Examples from left to right, erosion depth 0.4, 0.65 and 0.9

* Spherical (For worley noise only, added in release 5): Important setting that makes worley noise look billowy and puffy, you'll want this set to 1.0 for cumulus variations and convective clouds. Noise in previous versions would be the equivalent of this set 0.

![alt](https://images2.imgbox.com/21/40/FIPUeHR6_o.png)

Examples from left to right spherical 0.0, 0.5 and 1.0

* Brightness (Releases 1-4, removed in V5): Scales the value of the noise
* Contrast (Releases 1-4, removed in V5): The separattion between max and min values of the noise
* Lift (Releases 1-4, removed in V5): Raises the lowest values of the generated noise. Use if the noise has too many gaps and the gaps are eating your cloud texture.

There is no in-game preview of the noise generated but it will be added. For now you can preview it on the clouds themselves.

### Separate detail noise settings (added in Release 2, removed in release 5 as detail noise is now removed completely)

Separate noise settings can now be used for the detail noise, this is optional, if not used then the same base noise settings will be used. One advantage of this approach is being able to use Worley as base and Perlin as detail, or having negative contrast in the detail noise (creates a wispier look).

### Curl noise (added in Release 2)

This is curl noise that can be used to warp the other noises sampled. This can help produce a wispier and more "flowy" look. This may not work correctly with flowMaps.

![From left to right: No curl noise, increasing curl noise tiling and strength](https://i.imgur.com/cK12DFd.jpg)

Settings:

* Octaves and period: Properties of the noise that curl noise will be generated from

* Tiling: The size at which the curl noise will be tiled

* Strength: The strength of the displacement of curl noise applied to regular noises

* Smooth: When enabled it attempts to remove loops frm curl noise at high strengths, limited utility but can give a different look

* Contrast: Added in release 5

## CoverageMap, CloudTypeMap, CloudColorMap and FlowMap

See [this link](https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Raymarched-Clouds-Overview#coverage-map) for an explanation of what each map type does.

Cubemap type 2_1 (the one with 2 textures each containing a face in each RGB channel) isn't supported for now, equirectangular and cubemap with 6 textures are supported.

The same cubemap type needs to be used for all the textures on the same layer. Otherwise it won't work.

Alphamaps are supported and a different one can be used per texture.

CoverageMap will use the alpha channel by default if no alphamap is selected.

CloudTypeMap will use the red  channel by default if no alphamap is selected.

The detailTexture of the 2d layer can be reused with the coverageMap here (useful to create dynamic clouds), for that you need to check the useDetailTex option. This has a performance cost so isn't enabled by default.

Flowmaps are added in Release 2 and allow clouds to move locally, see the [related section](https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Flowmaps-(added-in-Release-2))

## SDFMap (Release 4)

The path to the SDF, see https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Signed-Distance-Fields-(Release-4)

## CloudTypes

A list of defined cloud-types, see [this link](https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Raymarched-Clouds-Overview#cloud-type-map) for an overview of what cloud types are.

You can add or remove CloudTypes in-game via the UI but it will only add or remove new types at the end of the list.

Settings:

* TypeName: Name of the cloud type, for you to keep track of the types, or for selecting the type in the in-game painter.

* MinAltitude and MaxAltitude: Defines the altitude ranges at which the volumetrics will appear. Volumetric layers that are active (see time settings) should not overlap or they will render wrong. Keep in mind that no matter which cloud type you're looking at, space will always be raymarched between the highest maxAltitude and lowest minAltitude, keep this in mind when configuring clouds as it will affect performance. A lot of vertical space raymarched will increase the workload, especially with small step sizes. Keep in mind that the altitudes of different volumetric layers that are active at the same time should not overlap, or else they render incorrectly (wrongly render behind/on top of each other).

* CoverageCurve:  A [float curve](https://forum.kerbalspaceprogram.com/index.php?/topic/84201-info-ksp-floatcurves-and-you-the-magic-of-tangents/) defining the "vertical profile" of the cloud, this defines the coverage (y axis) over height (x axis where 0 is the base of the cloud and 1 is the top). Can be used to create any cloud shape. If you're having trouble visualizing this mentally, put the curve sideways and you can see the shape of the cloud [as seen here](https://i.imgur.com/ercdoCH.png). Different shapes of different cloud types will be interpolated between them if you paint intermediary values on the cloud type map. See the CoverageCurve in effect in this video: [youtube](https://www.youtube.com/watch?v=ejpxSUyc8p8) There is no visual editor in the game for now, and the values are difficult to edit by text, so you can use waterfall's editor in-game or you can use one of these: [example](https://forum.kerbalspaceprogram.com/index.php?/topic/112317-18-amazing-curve-editor-13/), [other example](https://forum.kerbalspaceprogram.com/index.php?/topic/75187-unity-floatcurve-editor-10/)

* DensityCurve (Added in release 5):

* BaseNoiseTiling: The size at which the base noise will be tiled (see noise section above)

* DetailNoiseStrength (Removed in release 5): The strength of the detail Noise (see noise section above)

* InterpolateCloudHeights: If this cloud's curve should be interpolated with that of its neighbours when blending them. Check this if you have a specific shape which you don't want to get "warped" when mixing the cloud type with its neighbours.

* Density: How dense the cloud is. Smaller values should use bigger raymarching steps to keep performance. See raymarching settings quality section below. Additionally density may need to be adjusted for the cloud's scale. Super big clouds will look weird if they are too dense as lighting won't get inside them.

* Edge sharpness (Added in release 5):

* LightningFrequency (added in Release 2): Modulates the lightning spawn chance for this cloud type, see the related [config section](https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Lightning)

* AmbientVolume (added in Release 2): Modulates the volume of layer's ambient sound for this cloud type. See the related [config section](https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Raymarched-cloud-configuration#ambientsound-added-in-release-2)

* ParticleFieldDensity (added in Release 2): Modulates the particle field density for this cloud type, see the related [config section](https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Particle-fields)

* Droplets (added in Release 3): Droplets config to use for this cloud type, see the related [config section](https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Droplets)

## RaymarchingSettings

The volume is stepped through in discrete steps and evaluated. This is called raymarching, the smaller the step size, the higher the quality, the slower it will run. The bigger the step size, the faster it will be but it will become noisier and noisier. The goal of these settings is to find the sweet spot, as a general rule the smaller your noise scale the smaller the step sizes need to be to sample it. The lower the density the higher the step size can be. Adjust until you get good performance and no/less noise.

* BaseStepSize: The step size of the raymarching at the camera position.
* AdaptiveStepSizeFactor: How quickly to increase the step size when raymarching, this helps performance where less precision is needed in the distance.
* MaxStepSize: The maximum step size to use with the adaptive stepping configured above.

Additionally, inside the raymarch a secondary one is needed to check how much light reaches the cloud. The number of steps and max distance of which can be configured here, this affects the look of the cloud but also will cause noise if the number of steps is too low for the distance selected.

* LightMarchSteps: The number of samples used for the secondary raymarch
* LightMarchDistance: How far to raymarch

Keep in mind that the light volume (release 4) now handles long-distance shadows so these settings should only be used to add small-scale detail and the number of steps can be lowered.

* ContinuousAccumulationDistance (release 5):

## Phase Functions (Release 4)

Phase functions describe how light is scattered relative to the viewing the angle to the light source. Four phase functions can be defined here for customizing the look of the clouds.

Each phase function is a Vector where the first value is the eccentricity and the second is the brightness/strength. Use high eccentricity for strong forward scattering, low eccentricity for evenly distributed scattering and negative values for backscattering.

Two phase functions are provided for single scattering and two are provided for multiple scattering.

### Single scattering

Single scattering is very bright but doesn't penetrate deep inside the cloud and is very "harsh" looking, it is best to use it with high eccentricity for silver linings.

Example: Left is multiple scattering only, right is single+multiple scattering
![](https://i.imgur.com/s4LJwG2.png)

### Multiple scattering

Multiple scattering is not as bright but it penetrates very deep inside the cloud and has a very soft look which gives a fluffy appearance

Example: Left is single scattering only (brightened), right is single+multiple scattering

![](https://i.imgur.com/JspVfuI.jpg)

### Settings

* singleScattering1: Eccentricity and brightness for single scattering phase function
* singleScattering2: Eccentricity and brightness for single scattering phase function
* multipleScattering1: Eccentricity and brightness for multiple scattering phase function
* multipleScattering2: Eccentricity and brightness for multiple scattering phase function

## Light volume settings (Release 4)

* useLightVolume: Whether or not this layer should use the [light volume](https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Light-volume-(Release-4)) effects. You may want to disable this on invisible layers, or for artistic direction, on layers you don't want to be darkened by other layers above for example.
* maxLightVolumeRadius: The max radius used for light volume effects. You may want to limit this to maximize resolution near the camera or for other reasons. The min value between all visible layers will be picked. Light volume effects will softly fadeout towards the end of this radius and the old 2d cloud shadows will fade in. A default value of infinity is ok for stock scale and means everything will be rendered up to the horizon.

## Lightning (added in Release 2)

Optional, the name of a lightning config to use with the volumetric layer. See how to configure lightning [here](https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Lightning).

## ParticleField (added in Release 2)

Optional, the name of a particle field config to use with the volumetric layer. See how to configure particle fields [here](https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Particle-fields).

## AmbientSound (added in Release 2, updated in 3)

* SoundName: Optional, the path to a sound file to play inside the cloud layer, the volume will be modulated by the coverage, the coverage curve and the ambientVolume of the cloudType. The game accepts .ogg and .wav files, so I've been using .ogg
* IvaSoundName (added in Release 3): Optional, the path to a sound file to play inside the cloud layer when in IVA view, will be modulated in the same way as above. When this sound is present and when in IVA view, the regular sound will play at slightly lower volume and the IVA sound will play at a higher volume.