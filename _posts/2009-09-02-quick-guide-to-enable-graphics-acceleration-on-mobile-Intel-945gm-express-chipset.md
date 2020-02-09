---
layout: post
title: Quick guide to enable Graphics Acceleration on Mobile Intel® 945GM Express Chipset
tags: [Debian, Linux]
---

#### Checking if graphics acceleration already enabled

Required package:

* mesa-utls

```
# glxgears
524 frames in 5.0 seconds = 104.564 FPS
486 frames in 5.1 seconds = 95.546 FPS
540 frames in 5.0 seconds = 107.794 FPS
420 frames in 5.0 seconds = 83.957 FPS
480 frames in 5.1 seconds = 93.990 FPS
```

You can see the very poor rate of frames per second. It seems direct rendering is not enabled.

```
# glxinfo | grep direct
direct rendering: No (If you want to find out why, try setting LIBGL_DEBUG=verbose)
OpenGL renderer string: Mesa GLX Indirect
```

#### Enabling graphics acceleration

Required packages:

* xserver-xorg-video-i810

* libgl1-mesa-dri

```
# glxgears
3374 frames in 5.0 seconds = 674.634 FPS
3499 frames in 5.0 seconds = 699.728 FPS
2674 frames in 5.0 seconds = 534.604 FPS

# glxinfo | grep direct
direct rendering: Yes
```

Now, with direct rendering enabled, I get a high rate of frames per second.
