These are global raymarching quality settings that affect all layers. They can be changed from the in-game UI (ctrl+alt+0) from the RaymarchedCloudsQualityManager

![image](https://user-images.githubusercontent.com/11978271/212185961-a0f1ff13-e607-4c16-a809-5ea5faad6b47.png)

# Temporal upscaling

Temporal upscaling is the main optimization used for the raymarched clouds.

It speeds up the rendering of clouds by splitting up the rendering of a single frame over several frames. This is different from traditional upscaling because it eventually reconstructs a full frame (or a very close result to a full frame). Motion is adjusted for by reprojecting pixels from previous images to where they should be on the current frame.

Higher values are faster but have a lower quality. 8x (build a frame over 8 frames) and 9x are the sweet spot and are good for 1440p and 1080p. Lower resolutions may need to use lower values. 1x is native and is the slowest. Where 1x runs at ~20 fps on a decent GPU, 8x will run at 60-90 fps.

At 16x quality loss starts to become really noticeable. 32x is not recommended as the quality really suffers especially around the horizon.

In release 1 temporal upscaling will complain and the clouds won't work if the screen resolution is not divisible by the chosen number, this is fixed in release 2. 8x will divide the screen by 2 vertically and by 4 horizontally.

Left: fps when disabled, right enabled

![image](https://user-images.githubusercontent.com/11978271/212190889-b5d32552-1c3d-4922-b7b0-84d7b8db71a1.png)


# Noise detiling

Fixes tiling in the noise, performance intensive, enabled by default. Left: disabled. Right: enabled.

![image](https://user-images.githubusercontent.com/11978271/212186858-ebf1ce95-cf46-4c6c-bef8-53c9c870ef6a.png)
