By default the game loads all the textures at startup, which can result in heavy load times and added memory usage. Load on demand makes it so that textures are only loaded when needed.

## Using load on demand

To properly use load on demand, the game must not load the textures at startup, this can be done by placing the textures inside a "PluginData" folder, which you can create inside your mod directory inside GameData, everything else is the same.

## Compression formats to use

Load on demand supports .png (and .truecolor) and .dds, most of the common DDS formats are supported. BC4 support has been added in release 3 and is recommended for the highest quality grayscale textures (regular coverage/cloutype/maintex maps), BC5 has been added in release 5 and is recommended for normalmaps, you can use paint.net to export textures as DDS and BC4 DDS (make sure you vertically flip the texture before exporting, if you're converting a PNG to a DDS for the first time, otherwise they will be flipped in-game).

All the textures used in raymarched volumetrics (Coverage, CloudType, ColorMap, FlowMap) don't use mipmaps so no need to include them as they take up additional space.

Textures for 2D layers look best with sharp mipmaps (there's an option in paint.net for it).


| Use case                                      | Compression format to use              | MipMaps | EVE type  | EVE AlphaMask  |
|-----------------------------------------------|----------------------------------------|---------|-----------|----------------|
| 2D cloud texture with RGB color and alpha     | BC3 (DXT5)                             | Yes     | RGBA      | N/A            |
| 2D cloud texture with alpha only              | BC4 (Store alpha in R channel)         | Yes     | AlphaMap  | ALPHAMAP_R     |
| 2D cloud normalmaps                           | BC5                                    | Yes     | RGBA      | N/A            |
| 2D FlowMap                                    | BC5, or BC1 (DXT1) if you don't see artifacts, otherwise .png renamed to .truecolor                                     | Yes| RGBA      | N/A            |
| Volumetric Coverage                           | BC4                                    | No      | AlphaMap  | ALPHAMAP_R     |
| Volumetric CloudType                          | BC4                                    | No      | AlphaMap  | ALPHAMAP_R     |
| Volumetric ColorMap                           | BC1 (DXT1)                             | No      | RGBA      | N/A            |
| Volumetric FlowMap                            | BC1 (DXT1) if you don't see artifacts, otherwise .png renamed to .truecolor                             | No      | RGBA      | N/A            |

You might be tempted to use DXT1/DXT5 textures to pack multiple grayscale textures (eg coverage/type) in one texture to reduce texture size, then use ALPHAMAP_R, ALPHAMAP_G to extract the textures for different textures but that's a bad idea in practice:
* The compression artifacts will slightly mess up your cloud type or coverage map and you might not realize it at first but your cloud formations will look weird (happened to me with anvil clouds)
* Using a format with more channels and more bits per pixel than is needed (like DXT5 being 8-bit per pixel vs BC4 being 4-bit per pixel) will make the raymarching shader do more work and read more data per pixel and will run **slower**, this can be pretty noticeable up to 10% performance difference on the whole game