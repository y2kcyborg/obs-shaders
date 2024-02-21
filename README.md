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

# Noise Mask / Invisible When Paused

Inspired by this video and its follow-ups, but made more eye-safe:
https://www.youtube.com/watch?v=TdTMeNXCnTs (Heed the flashing warnings on the follow-ups!)

The original has randomly bouncing lines drawn with XOR over an accumulating frame buffer.
After some time, the frame buffer is effectively random noise, and looks like nothing when paused,
but when a line is drawn the eye can pick up the changed pixels and generates an illusion of motion.

There are some follow-up videos rendering Bad Apple with this technique, but for large solid areas to
be visible they have to be flashing every other frame which is some of the worst flickering I've seen in video.

This shader does something a little simpler. If we start from the idea that we want an image full of noise,
with no edges visible when paused, but with the actual noise values changing with scene motion, then another
solution is to mask between two different but statistically indistinguishable noise fields.

The disadvantages are that only edges in motion will be visible, and not solid areas; and that
only on average half of the pixels along edges will change, as opposed to all of them.

The advantages are that it's much easier on the eyes, and it's cheap! You could even do it by masking between
two suitable images, using the a Dynamic Mask or similar filter.

Here, I provide a version with some configurable noise parameters.
The filter should be applied to a source that is alpha masked - this could come from a chroma key,
a video with alpha, or a vtuber model from an app with Spout and transparency enabled.