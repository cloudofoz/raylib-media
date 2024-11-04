![Version](https://img.shields.io/badge/raylib--media-v0.1beta-informational) ![Version](https://img.shields.io/badge/raylib-v5.0-informational) ![License](https://img.shields.io/github/license/cloudofoz/raylib-media)

> **Note**: This library is in **beta**. Your feedback and support in enhancing its quality are greatly appreciated!

## Introduction

**raylib-media** is a clean and user-friendly *extension* for [raylib](https://www.raylib.com/) that adds seamless audio and video streaming support via the [FFmpeg](https://ffmpeg.org/about.html) **libav\*** libraries.
It enables easy integration of multimedia content into raylib applications, providing direct access to video textures and audio streams, with support for seeking and looping.

<p align="center">
  <img src="res/rmedia_icon.svg" alt="raylib-media icon" width="270" height="270">
  <img src="res/raylib_example_01.gif" alt="raylib-example">
</p>

## Core Features

- Simple yet effective, with customizable options
- Direct access to video `Texture` and `AudioStream` for efficient media handling
- Optimized memory usage: no direct allocations outside the `LoadMedia` function
- Synchronized audio and video playback
- Supports media seeking and looping
- Compatible with formats supported by the codecs in the linked FFmpeg build

## Minimal Usage

These 3-4 lines of code show the minimal code needed to play a video with `raylib-media`:

```c
#include <raymedia.h>

MediaStream media = LoadMedia("path/to/your_file.mp4"); // Load the media

while (...) { // Begin your main loop
    ...
    UpdateMedia(&media); // Update the media stream according to frame time
    ...
    DrawTexture(media.videoTexture, 0, 0, WHITE); // Draw the video frame
    ...
}

UnloadMedia(&media); // Unload media when done
```

---

## Dependencies

`raylib-media` depends on the following files and libraries (a build system is not yet available):

1. **`src/raymedia.h`** and **`src/rmedia.c`**

   - You can include them directly in your project or compile **`rmedia.c`** and use the compiled library.

2. **[raylib](https://www.raylib.com/)**

   - Since **raylib-media** is an extension of **raylib**, it's assumed you are already using it and know how to compile it. This can easily be done using CMake or one of the available project files.

3. The following subset of **libav\*** libraries from **[FFmpeg](https://www.ffmpeg.org/)**:

   - **`libavcodec`**
   - **`libavformat`**
   - **`libavutil`**
   - **`libswresample`**
   - **`libswscale`**

   You may want to start by using precompiled libraries and later compile your own version, stripping out the codecs you don't use to reduce the library size and tailor it to your needs:

   - **Linux** [*Need testing*]: Install via your package manager (e.g., `sudo apt-get install libavcodec-dev libavformat-dev libavutil-dev libswresample-dev libswscale-dev`).
     
   - **macOS** [*Need testing*]:
     Use Homebrew (`brew install ffmpeg`).
     
   - **Windows**:
     Download compiled libraries from sources like [FFmpeg Builds by BtbN](https://github.com/BtbN/FFmpeg-Builds): [`ffmpeg-n7.1-latest-win64-lgpl-shared-7.1.zip`](https://github.com/BtbN/FFmpeg-Builds/releases/download/latest/ffmpeg-n7.1-latest-win64-lgpl-shared-7.1.zip)



---

## About FFmpeg

FFmpeg is available in two versions:

- The complete version under a **GPL** license.
- A more permissive version without certain proprietary codecs under an **LGPL** license.

**What does this mean for you?**

- **LGPL Version**: If you prefer more flexibility in licensing your own code, choose the LGPL version. By linking LGPL **libav\*** libraries dynamically, you're free to license your code as you wish without additional obligations.
- **GPL Version**: Using the GPL version requires that your code also be released under the GPL license, which mandates that the source code be made available under the same terms.


---
  
## Credits

Special thanks to the following resources:

- [FFmpeg Libav Tutorial](https://github.com/leandromoreira/ffmpeg-libav-tutorial) - This resource was invaluable in helping me start to dive into FFmpeg and Libav.
- [FFmpeg Builds by BtbN](https://github.com/BtbN/FFmpeg-Builds) - For providing compiled dependencies that are easy and straightforward to use, perfect for immediately starting to use **raylib-media**.


<br>

### *Documentation, instructions, and examples will follow (WIP)*

