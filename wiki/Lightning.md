Lightning can now be added to cloud layers, it's configurable with a bolt texture, emitted light that affects scenery and clouds, as well as near and far sounds.

![](https://i.imgur.com/My6PLSd.png)

Lightning configs are separate from the cloud layer config, the lightning config is then referenced in the volumetric cloud config. Lightning has a separate manager and config nodes.

Due to using lights which can be performance-intensive in KSP (due to forward rendering) the number of simultaneous lightning strikes is limited, so spawn chance will appear to be soft-capped.

Lightning is spawned at random times using the spawnChancePerSecond variable, when lightning is about to be spawned, a random position is generated and if a cloud is there (by checking the coverage and coverageCurves) lightning will be spawned. Then the spawn chance is further modulated by the cloudtype's lightningFrequency. This allows different types (fog, rain, heavy rain) etc to handle lightning separately in the same layer. The bolt is cast downwards from the spawn point with configurable dimensions.

## Settings

* name: The lightning config name, you will use this to reference a lightning config in a volumetric layer config

***

* spawnChancePerSecond: The spawn chance per second for a lightning bolt. This is then further modulated by the lightningFrequency of the cloud type. Note that this is a "chance" and exact spawn times are randomized. 
* minSpawnRange: The spawn range of the lightning is increased with the camera altitude, and decreased when closer to the planet surface so as to maximize the number of visible lightning strikes, this is the minimum spawn range that will be used when you get closer to sea level.
* spawnAltitude: The altitude at which the lightning will be spawned, this is the altitude at which the coverage and coverageCurve will be checked for spawning the lightning, so this needs to match what you have in the volumetric layer. In meters.

***

* lifeTime: How long the lightning strike lives, bolt transparency and light intensity will fall off during this lifetime. In seconds.
* lightColor: The color of the emitted light
* lightIntensity: The brightness of the emitted light
* lightRange: The range of the emitted light, in meters.

***

* boltWidth: The width of the spawned lightningBolt, in meters
* boltheight: The height of the spawned lightningBolt, in meters
* boltTexture: The texture sheet of lightningBolts
* lightningSheetCount: The horizontal and vertical counts of textures in the bolt sheet, a random texture from the sheet will be taken at every strike
* boltColor: Color of the spawned bolt

***

* soundMinDistance: Sound is modulated by the distance, this is the distance from the camera where sound is at its loudest, in meters.
* soundMaxDistance: Sound intensity will falloff in between the minDistance and maxDistance, beyond maxDistance no sound is audible, in meters.
* nearSounds: A list of sounds that will be played close to the camera, I use loud cracks for these. The game seems to accept .ogg and .wav files so I've been using .ogg
* farSounds: A list of sounds that will be played far from the camera, I use deep thunder for these
* soundFarThreshold: The distance from the camera beyond which far sounds will be used, in meters.
* realisticAudioDelayMultiplier: Set to 1 for realistic audio delay, disabled by default on my configs because it's confusing and just looks random and feels decorrelated from the lightning, the large distances in KSP certainly don't help this.

## Adding lightning to a volumetric cloud layer

To add lightning to a volumetric layer, enable the lightning node and reference the name of your config (as seen above).