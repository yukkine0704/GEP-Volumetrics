The light volume is a system for handling large scale cloud shadows and ambient. It is shared between all active layers that use it. Layers have [separate opt-out settings](https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Raymarched-cloud-configuration#light-volume-settings-release-4) to skip using it if needed.

When porting old configs you'll want to reduce the number of light raymarching steps and distance, because the light volume can now handle the longer distance ones. This helps offset the (small) performance cost of using the light volume. You'll want to keep some light raymarching steps for small scale detail though, although on some effects like fog and rain the light raymarching steps can be completely removed and replaced with light volume.

Examples: Left lightVolume is disabled (shading as it appeared in previous versions), middle is with light volume shadows only, right is with lightvolume shadows+ambient

![](https://i.imgur.com/YI43LAj.png)

Global settings affect its resolution and update rate, they can be changed from the in-game UI (ctrl+alt+0) from the RaymarchedCloudsQualityManager.

The light volume is also used by scatterer to create [godrays](https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Raymarched-Clouds-Overview#atmospheric-godrays-added-in-release-4)

# Settings

* horizontalResolution: The horizontal resolution of the light volume, this will be used to cover all the clouds up to the visible horizon, detail is concetrated around the camera and lower in the distance. This is recentered dynamically based on movement/altitude.

* verticalResolution: The vertical resolution or number of slices in the light volume, this will be distributed to cover all altitudes from the lowest altitude of the lowest layer using light volume, to the highest altitude of the highest layers. Rescales dynamically when the layers move/enable/disable.

* stepCount: The number of raymarching steps used to resolve the long-distance shadows and ambient used in the light volume.

* directLightTimeSlicing: Over how many frames to render the shadows. Lower is better but more performance-intensive.

* ambientLightTimeSlicing: Over how many frames to render the ambient. Lower is better but more performance-intensive. This is more intensive than the shadows, but also less noticeable if it doesn't update as often.

* timewarpRateMultiplier: Multiplier to increase the update rate of direct and ambient in time warp.