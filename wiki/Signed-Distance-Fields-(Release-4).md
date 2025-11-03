## Overview

Signed Distance Fields are textures where each texel stores the distance to the nearest surface. They are a way to skip empty space when raymarching, for better performance.

They are optional.

The current implementation only skips the empty areas in the coveragemap, that means that space above a cloud within the same layer cannot be skipped.

## Performance

In practice performance is currently not improved a lot with SDFs except in certain situations:
* The cloud layer is very sparse and has a lot of empty space (for example Laythe volcanoes)
* The cloud layer is very sparse and very tall (for example Laythe volcanoes if they were made even taller)
* The raymarching step size is very small

It is best to do comparisons with and without them, sometimes they may hurt performance.

## Usage

Go to the [cloud painter](https://github.com/LGhassen/EnvironmentalVisualEnhancements/wiki/In-game-Cloud-Painter) and press generate SDF.

Assign your SDF path in the sdfMap field in the layerRaymarchedVolume node (without .sdf extension).