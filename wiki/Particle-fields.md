Particle fields are designed to handle lots of small particles efficiently on the GPU, for use in environmental effects such as rain, snow and dust. They render in a small area around the camera. They receive light from player light sources.

![](https://i.imgur.com/2JLWS65.png)

Particle field configs are separate from the cloud layer config, the Particle field config is then referenced in the volumetric cloud config. Particle fields have a separate manager and config nodes.

The particle field density will be modulated by the cloud's coverage map and coverage curve at the current camera position. Additionally every cloud type has a ParticleFieldDensity variable to further modulate it.

Particles can be made to stretch in their movement direction, this is perfect for making rain. When the craft is moving they will stretch more in the opposite of the craft's movement direction.
Splashes can also be added for contact with surfaces.

Only one particle field can be added to a volumetric layer, but a trick to have multiple is to add a second volumetric layer with 0 density and very high step sizes (that way it doesn't cost anything to render). I use this trick to add both snow and rain to the main layer of Kerbin.

## Settings

* name: The particle field config name, you will use this to reference a lightning config in a volumetric layer config

***

* fieldParticleCount: How many particles the field will contain.

* fieldSize: The distance in front of the camera in which the particles will be rendered, the particle count will be spread around this space.

* particleSize: The size of the individual particles, in meters, if the particles don't use any stretch.

* particleStretch: A multiplier to stretch the particle length along it's movement direction.

* minCoverageThreshold: The coverage value below which no particles will appear

* maxCoverageThreshold: The coverage value at which particles reach maximum density

***

* fallSpeed: Fall speed of particles in m/s

* tangentialSpeed: Tangential movement speed of particles, the tangent direction is the direction that the cloud layer moves in. This feature is a bit glitchy and jittery at the moment so don't use it at low tangential speeds. At high speeds it isn't noticeable. In m/s

* randomDirectionStrength: Amount of randomization to each particle's movement direction.

***

* particleTexture: The texture sheet of the particles.

* particleSheetCount: The horizontal and vertical counts of textures in the particle sheet, a random texture from the sheet will be used for every particle.

* color: The color of the particle.

***

* Splashes: Optional, mostly identical to the above settings, but will be used for separate "splashes" particles which will only appear when in contact with solid surfaces. Used for rain splashes as the name implies. If set to the same fall speed of rain, splashes will appear exactly where particles hit surfaces but they will be short-lived due to technical limitations. If set to a different, lower speed, they won't match the rain particle positions but they will look more natural. The decorrelation doesn't seem to be an issue in practice.

## Adding particles to a volumetric cloud layer

To add particles to a volumetric layer, enable the particle field node and reference the name of your config (as seen above).