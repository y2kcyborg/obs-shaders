# Millennium Cyborg's OBS shaders

To use the shaders in this repository, you'll need an OBS plugin - either obs-shaderfilter or StreamFX.

Currently, the shaders here are compatible with both.

obs-shaderfilter:
https://github.com/exeldro/obs-shaderfilter/releases/tag/2.1.3

StreamFX:
https://github.com/xoxfaby/obs-StreamFX/releases/tag/0.12.0b299

# Dither / FMV shader

Early Macromedia FMV games used Quicktime 1.0 and the RPZA video codec.
This shader processes the image in a way similar to FFmpeg's RPZA encoder:
https://ffmpeg.org/doxygen/trunk/rpzaenc_8c_source.html

The image is reduced to 5-bit RGB with a 4x4 ordered dither.
Based on error thresholds you can modify, 4x4 image blocks are then
encoded as constant colour, a 4-colour palette along a gradient,
or left as the full 16 pixel colours.

This version does NOT include the option of a 4x4 block being reused across
frames like in RPZA.

I recommend applying this to a scene rather than an individual source - or if you apply
it to a source, make sure it's at its original size. Otherwise the pixel scale might be off!