This section describes how to configure planets, it is aimed at modders and advanced users.

- [Overview](#overview)
- [Configuration](#configuration)
  * [Planets list](#planets-list)
  * [Atmosphere configuration](#atmosphere-configuration)
    + [Config format](#config-format)
      - [Global settings](#global-settings)
      - [Config points](#config-points)
    + [Generating precomputed atmospheric scattering tables](#generating-precomputed-atmospheric-scattering-tables)
  * [Ocean configuration](#ocean-configuration)

# Overview

Planets in Scatterer require a global planets list file, which configures the general settings for planets, per-planet config files for the atmosphere and ocean, and precomputed atmospheric scattering tables for each atmosphere. We will go over each of these, however note that to generate the precomputed atmospheric scattering tables, a machine with DirectX 11 support is needed.


# Configuration
## Planets list

The planets list file serves as a "master list" and holds the general settings for each planet. If your planet is not on this list it will not have scatterer effects on it.

The game’s configNode system is used, a planetsList file is included with the stock config for stock planets, but it can be overridden and modified with ModuleManager patches.

The planets list file has the following structure

	Scatterer_planetsList
	{
		scattererCelestialBodies
		{
			Item
			{
				key = value
				...
				secondarySuns
				{
					Item
			                {
				                key = value
				                ...
			                }
			                ...
			        }
			}
        	}
        	sunflares
        	{
        		...
        	}
   	 }

Key values to use in the planetsList file:

| Variable | Description| Example|
| ----------| ---------- | ----- |
| celestialBodyName | Name of the celestial body to apply the effect to | "celestialBodyName = Kerbin" |
| transformName | The transform name of the body, typically this should be the same as the celestialBodyName, but this is not the case for some modded planets (to recheck if this is still useful) | "transformName = Kerbin" |
| loadDistance | Distance from the camera at which the effects should be loaded for this body, in meters. | "loadDistance = 100000000" |
| unloadDistance | Distance from the camera at which the effects should be unloaded for this body, in meters. | "unloadDistance = 200000000" |
| hasOcean | If the planet has an ocean. When this is enabled and the ocean shaders are enabled, an ocean config will be loaded for the planet. | "hasOcean = True" |
| flatScaledSpaceModel | If the scaled space planet (the planet as seen from high orbit or map view) should have flat, uniform atmospheric scattering, or the scattering should respect the planet's features (mountains etc), see comparison: ![](https://i.imgur.com/qHxcgPZ.jpg) Also, in some cases where the planet's model does not have enough detail, surface polygons can be shaded as features, which makes the polygonal nature result in very apparent artifacts: ![](https://i.imgur.com/bNTTCe2.jpg) in this case enable flat shading to avoid them. | "flatScaledSpaceModel = True" |
| usesCloudIntegration | If the planet should show scattering and extinction effects on it's EVE clouds (if they are available) | "usesCloudIntegration = True" |
| cloudIntegrationUsesScattererSunColors | When set to false, will use the real sun colors for clouds (for main sun and secondary suns), when true will use Scatterer's sun colors, defaults to false. | "cloudIntegrationUsesScattererSunColors = False" |
| mainSunCelestialBody | The name of the celestial body of the sun lighting this planet. | "mainSunCelestialBody = Sun" |
| sunColor | Color of sunlight, can be modified for colored suns, will affect the scattering look. | "sunColor = 1.0,1.0,1.0" for standard white sunlight |
| sunsUseIntensityCurves | When set to true, both main sun and secondary suns will use the intensity from the star's intensity curve. Useful for planets on elliptical orbits. Defaults to false for retro-compatibility. | "sunsUseIntensityCurves = True" |
| eclipseCasters | A list of bodies that can cast eclipses on this planet, Items should be separated by a line break. | eclipseCasters { Item = Mun } |
| secondarySuns | A list of secondary suns that can light up the planet. Secondary suns don't cast shadows, eclipses, godrays, cloud shadows and caustics. Each secondary sun has a name and sunColor, maximum of 4 secondary suns | 			secondarySuns\{ Item { celestialBodyName = Tatoo I sunColor = 0.9,0.8,0.7 } Item { celestialBodyName = Tatoo II sunColor = 0.7,0.6,0.5 } } |

## Atmosphere configuration

The game’s configNode system is used, you can add a .cfg file, or use ModuleManager patches.

All of these settings can be changed in-game from the scatterer UI (alt+f10/f11) apart from the settings related to the precomputed atmospherc scattering tables. Changes will be lost on scene changes unless you manually press the save button or enable the option "autosavePlanetSettingsOnSceneChange" from the general scatterer config, see: https://github.com/LGhassen/Scatterer/wiki/GeneralConfig#autosaveplanetsettingsonscenechange

Note that you can't save changes to settings applied via ModuleManager patches from the UI.

### Config format

An atmosphere config file has the following structure

	Scatterer_atmosphere
	{
		Atmo
		{
			key = value
			...
		
			configPoints
			{
				Item
				{
					key = value
					...
				}
			}
		}
	}

It has some global settings and a list of "configPoints" which will be explained below.

#### Global settings

The embedded images can be enlarged with right click -> view image

| Variable      | Description | Example |
| ------------- |-------------  |-------|
| name      | Name of the celestial body, has to match the name entered in the planetsList file     | "name = Kerbin"
| assetPath | The path under GameData where the precomputed inscattering tables are located, nothing will work without this     | "assetPath = scatterer/config/Planets/Kerbin/Atmo"
| atmosphereGlobalScale          | Scales the atmosphere "whole cloth", including the radius where atmosphere starts. As if the atmosphere was applied to a bigger or smaller planet. Not recommended not modify. In previous versions of KSP this was useful where a planet's model was slightly smaller than the radius but this doesn't seem to be the case anymore. Comparison: Values of 1 and 1.02 ![](https://i.imgur.com/IuMdAY7.jpg)  | "atmosphereGlobalScale = 1"
| experimentalAtmoScale          | Scales the atmosphere height without scaling the planet radius. Scaling atmospheres via the config tool can be finnicky and require regenerating the atmosphere every time, this setting can be used to scale the atmosphere without regenerating and allow previewing directly in-game. Essentially this rescales Rt (see below) such that Rt = Rt + (Rt-Rg) * experimentalAtmoScale. Comparison: Values of 2 and 10 ![](https://i.imgur.com/QmZLWwr.jpeg)          | "experimentalAtmoScale = 1"
| flattenScaledSpaceMesh          | The scaled space planet is what you see from high orbit or map view, it is rendered "behind" the actual terrain. The problem is the terrain and the scaled space planet don't always match, causing the scaled planet to be visible behind terrain edges, which can result in this kind of artifact (enlarge image): ![](https://i.imgur.com/YzxZYt1.jpg) or this kind of artifact: ![](https://i.imgur.com/xlS70ol.jpg) Use this setting to flatten the scaled planet so it's hidden, 0 means no change, 1 makes the scaled planet a flat sphere | "flattenScaledSpaceMesh = 0.6" for the stock Kerbin
| cloudColorMultiplier          || 
| cloudScatteringMultiplier          || 
| cloudSkyIrradianceMultiplier          || 
| EVEIntegration_preserveCloudColors          || 
| volumetricsColorMultiplier          || 
| godrayStrength          | The strength of godrays, or how much light is occluded in the godrays. 1 will make godrays too dark, 0 means godrays will not be visible. Adjust to taste. Comparison: 0.9 vs 0.5 vs 0.25 ![](https://i.imgur.com/4nU8nkt.jpg) | 
| godrayCloudAlphaThreshold|| 
| specR       | |
| specG       ||
| specB       ||
| shininess       ||
| rimBlend          ||
| rimpower          ||
| Rg      | Radius of the planet used to generate the precomputed inscattering tables (refer to the section "Generating precomputed atmospheric scattering tables"). The value here isn't important, but ratio of Rt/Rg is. Upon atmosphere load the Rt and Rg will be scaled to the new planet. This way you can use for example the same atmosphere used for Kerbin on Earth without any adjustments | "Rg = 600000"
| Rt      | Radius of the atmosphere (Rg + atmosphere height), the ratio of Rt/Rg should match what was used in the config tool (refer to the section "Generating precomputed atmospheric scattering tables")      | "Rt = 608490.375"
| RL      | RL = Rt + epsilon, only used in the config tool, not in-game. May be saved here for reference in case you need to regenerate an atmosphere| "RL = 609000"
| BETA_MSca      | Scatter coefficient for mie (has to match what is used in the config tool)   | "BETA_MSca = 0.004,0.004,0.004"
| m_betaR       | Scatter coefficient for rayleigh  (has to match what is used in the config tool)     |  "m_betaR = 0.0058,0.0135,0.0331"
| m_mieG       | Asymmetry factor for the mie phase function (can be adjusted in-game)     |  "m_mieG = 0.78"
| HR       | Half height for the atmosphere air density (has to match what is used in the config tool)     | "HR = 10"
| HM       | Half height for the particle density (has to match what is used in the config tool)     |  "HM = 1.20000005"

#### Config points

Config points are a mechanism that allows to have different settings per altitude. This allows some settings to be tweaked at specific altitudes to target certain artifacts or to achieve a different look on the surface and from orbit for example.

Each config "point" is defined by it's altitude and the settings applied at that altitude. If you don't wish to use this just define a singular config point. Config points should be input in the correct altitude order.

When below the altitude of the first point, the settings values of that point will be used as they are. When between two points, the settings used will be an interpolation of both. Example, if point 0 is set at 1000m and point 1 is set at 5000m, the values used by scatterer at a 2000m altitude will be 75% of point 0 + 25 % of point 1. When passed the altitude of the highest point, the values of that point are used as they are. These are also used in map view.

You can browse between the config points, delete and add new ones in the UI. Always make sure to have them properly ordered by altitude. You can also view the current values in the UI and how they are computed.

| Variable      | Description | Example |
| ------------- |-------------  |-------|
| altitude| The altitude of the given config point     | "altitude = 200"
| skyExposure|    The exposure/brightness of the sky inscattering. Essentially the sky color is multiplied by this value before the tonemapping operation is applied on the sky, resulting in a natural feeling brightness Comparison: Brightness 0.1 vs 0.25  ![](https://i.imgur.com/exGiVJc.jpg)   | "skyExposure = 0.25"
| skyAlpha| Used to be a true alpha value, but since the switch to additive blending for the sky, this is now the same as skyExposure but applied after the tonemapping operation. Limited usefulness, but may behave a bit better than exposure for making dim/thin atmospheres | "skyAlpha = 1"
| skyExtinctionTint|  Before we proceed, a small reminder on atmospheric scattering. Refer to the following diagram: ![](https://i.imgur.com/wkHNFAv.png) ![](https://i.imgur.com/8T6xTbD.png) ![](https://i.imgur.com/wkHNFAv.png) When light hits an atmosphere particle, a part of it is scattered away, we call this "outscattering". What remains of the incident light reaches the viewer, but it is different, less intense, and of a different color, as some of it has been scattered out, we say that this incident light has experienced "extinction". In the example of earth, the atmosphere scatters blue light, so what direct sunlight reaches us appears to be more red, especially during sunset when this incident light travels longer through the atmosphere and more of it is scattered. In the same way that light is scattered away, light from a completely different direction can be scattered towards the viewer, this is called inscattering ![](https://i.imgur.com/wkHNFAv.png)  ![](https://i.imgur.com/Pu2la34.png) ![](https://i.imgur.com/wkHNFAv.png)  If we disable sky scattering, we can see what the extinction does:  ![](https://i.imgur.com/wkHNFAv.png) ![](https://i.imgur.com/1ouD625.jpg)  ![](https://i.imgur.com/wkHNFAv.png) It appears as though the atmosphere is dimming what is directly behind it, and imparting a tint on it. This parameter (skyExtinctionTint) allows tweaking this tint. 1 is the normal tint, 0 means no tint. Comparison with values of 1, 5 and 0 respectively ![](https://i.imgur.com/wkHNFAv.png) ![](https://i.imgur.com/6n35mB5.jpg) | "skyExtinctionTint = 1"
| scatteringExposure| The exposure/brightness of the inscattering on terrain (in both local and scaledSpace), may need to be a bit different from skyExposure so that the brightness on the sky and the terrain match  | "scatteringExposure = 0.23"
| extinctionTint|  Same as the skyExtinctionTint but for terrain (in both local and scaledSpace)    |  "extinctionTint = 0.5"
| extinctionThickness |   Where extinction tint changes only the tint, this modifies the intensity of the effect. This parameter applies to the sky extinction, the terrain extinction and the sunlight extinction. Comparison    | "extinctionThickness = 1"
| postProcessAlpha|       | ""
| postProcessDepth|       | ""
| viewdirOffset|       | ""
| openglThreshold|       | ""

### Generating precomputed atmospheric scattering tables

Since version 0.0824 you can just generate the atmosphere directly in-game, from the atmosphere tab in the UI.

Previous versions need to use the config tool to export atmosphere files.

## Ocean configuration