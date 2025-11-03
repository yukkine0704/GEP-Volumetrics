## Overview

An in-game cloud painter can be accessed from the UI via the CloudPainterManager.

It can paint to cloud coverage, cloud type, cloud color, flowmaps and export textures.

When painting cloud coverage and types in-game, lightning and particles won't be able to "see" the new changes until the textures are saved to disk and reloaded in a game restart.

## Usage

Open the UI with ctrl + alt + 0, then navigate to CloudPainterManager with the top right arrow.

Now just select the cloud layer you want and press paint.

![image](https://user-images.githubusercontent.com/11978271/212187512-7c62ab38-d16b-4a06-80c8-edb682a4f8b8.png)

Select a mode enter the brush values you want and start painting

When done you can save textures (they will be saved to GameData/EVETextureExports) or you can reset them to how they originally were.

You can also generated [Signed Distance Fields](https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/Signed-Distance-Fields-(Release-4)) from this screen.

## A note about editing and saving textures

The game always compresses uncompressed textures when starting up, so if you use the in-game textures to paint and then save them you will accumulate compression artifacts. To avoid this you can rename your .png files to .truecolor and they will be loaded uncompressed, you can then edit them in-game without quality loss. When done you can rename them back to .png or convert to .dds

## Painting modes

* coverageAndCloudType
* coverage
* cloudType
* colorMap
* flowMapDirectional: Click and drag to paint flow in a given direction, can be fiddly
* flowMapVortex: Click to paint a flow vortex, can set direction (cw/ccw)
* flowMapBand: Paints a whole band in a given direction around the planet
* scaledFlowMapDirectional: Same as flowMapDirectional but for scaledFlowmap, you should use map view to paint this
* scaledFlowMapVortex: Same as flowMapDirectional but for scaledFlowmap, you should use map view to paint this
* scaledFlowMapBand: Same as flowMapBand but for scaledFlowmap, see video below for example, you should use map view to paint this

## Limitations
* Will create the same resolution textures as the textures you start with
* If a texture type isn't assigned to the layer before painting you will not be able to paint in that mode
* When painting cloud coverage and types in-game, lightning and particles won't be able to "see" the new changes until the textures are saved to disk and reloaded (you can export the textures, unload the painter and asign new textures).

## Videos:

[![ScaledFlowmapBands](https://i.ytimg.com/vi/b5TPm_7yqOE/hqdefault.jpg)](https://www.youtube.com/watch?v=b5TPm_7yqOE&ab_channel=blackrack)

[![Coverage and cloudType](https://i.ytimg.com/vi/a96inAa04q4/maxresdefault.jpg)](https://www.youtube.com/watch?v=a96inAa04q4)