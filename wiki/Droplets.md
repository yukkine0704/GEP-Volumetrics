When in IVA view rain droplets will now be visible through transparent parts on the outside of the craft (eg glass, cockpits).

![](https://i.imgur.com/mUEIo6O.png)

Droplets work on every part and don't require per-part configs or maps. When in motion the direction, speed and "streakiness" of the droplets will change accordingly.

## Settings

The droplets have global settings that control the shading of the droplets, and layer settings that allow to add multiple layers of droplets that can have different sizes, move at different speeds etc

* name: The droplet field config name, you will use this to reference a droplet config in a volumetric layer config.

***

* minCoverageThreshold

* maxCoverageThreshold

* fadeOutStartSpeed

* fadeOutEndSpeed

* refractionStrength

* translucency

* color

* specularStrength

* scale

* triplanarTransitionSharpness

* dryingSpeed

* speedIncreaseFactor

* maxModulationSpeed

* noise

* sideNoiseScale

* sideLowSpeedNoiseStrength

* sideHighSpeedNoiseStrength

* lowSpeedStreakRatio

* highSpeedStreakRatio

* lowSpeedTimeRandomness

* highSpeedTimeRandomness

* topNoiseScale

* topNoiseStrength

* sideDropletLayers

* * name

* * fallSpeed

* * scale

* * dropletToTrailAspectRatio

* * streakRatio

* topDropletLayers

* * name

* * scale

* * speed

## Adding droplets to a volumetric cloud layer

To add droplets to a volumetric layer, enable the droplets node and reference the name of your config (as seen above).