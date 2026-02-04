---
name: video-audio
description: Creatomate video concatenation, trimming, picture-in-picture, audio tracks, and transcoding
---

# Video & Audio Elements

Working with video and audio files in Creatomate.

## Video Element

### Basic Video

```javascript
new Creatomate.Video({
  source: 'https://example.com/video.mp4',
})
```

### Video with Options

```javascript
new Creatomate.Video({
  source: 'https://example.com/video.mp4',

  // Position & Size
  x: '50%',
  y: '50%',
  width: '100%',
  height: '100%',

  // Fit behavior
  fit: 'cover',  // 'cover', 'contain', 'fill'

  // Timing
  time: 0,           // Start time in output
  duration: null,    // null = use video length

  // Trimming (cut from source)
  trimStart: 2,      // Skip first 2 seconds
  trimDuration: 10,  // Use 10 seconds of video

  // Audio
  volume: '100%',
  muted: false,
  audioFadeIn: 0,
  audioFadeOut: 0,

  // Playback
  playbackRate: 1.0,  // Speed: 0.5 = half, 2.0 = double
  loop: false,
})
```

## Concatenating Videos

Place videos on the same track for sequential playback:

```javascript
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  width: 1280,
  height: 720,

  elements: [
    new Creatomate.Video({
      track: 1,
      source: 'https://example.com/video1.mp4',
    }),

    new Creatomate.Video({
      track: 1,
      source: 'https://example.com/video2.mp4',
      // Add transition
      transition: new Creatomate.Fade({ duration: 1 }),
    }),

    new Creatomate.Video({
      track: 1,
      source: 'https://example.com/video3.mp4',
      transition: new Creatomate.Fade({ duration: 1 }),
    }),
  ],
});
```

## Trimming Videos

```javascript
// Trim to specific portion
new Creatomate.Video({
  source: 'https://example.com/long-video.mp4',
  trimStart: 10,      // Start at 10 seconds
  trimDuration: 30,   // Use 30 seconds
})
```

## Picture-in-Picture

```javascript
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  width: 1920,
  height: 1080,

  elements: [
    // Main video (full screen)
    new Creatomate.Video({
      source: 'https://example.com/main.mp4',
    }),

    // PiP video (small, corner)
    new Creatomate.Video({
      source: 'https://example.com/pip.mp4',
      x: '85%',
      y: '15%',
      width: '25%',
      height: '25%',
      borderRadius: '10px',
      shadow: {
        color: 'rgba(0,0,0,0.5)',
        blur: '10px',
      },
    }),
  ],
});
```

## Split Screen

```javascript
// Side by side
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  width: 1920,
  height: 1080,

  elements: [
    // Left video
    new Creatomate.Video({
      source: 'https://example.com/left.mp4',
      x: '25%',
      width: '50%',
      height: '100%',
    }),

    // Right video
    new Creatomate.Video({
      source: 'https://example.com/right.mp4',
      x: '75%',
      width: '50%',
      height: '100%',
    }),
  ],
});
```

## Video Wall (2x2)

```javascript
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  width: 1920,
  height: 1080,

  elements: [
    // Top-left
    new Creatomate.Video({
      source: 'https://example.com/v1.mp4',
      x: '25%', y: '25%',
      width: '50%', height: '50%',
    }),
    // Top-right
    new Creatomate.Video({
      source: 'https://example.com/v2.mp4',
      x: '75%', y: '25%',
      width: '50%', height: '50%',
    }),
    // Bottom-left
    new Creatomate.Video({
      source: 'https://example.com/v3.mp4',
      x: '25%', y: '75%',
      width: '50%', height: '50%',
    }),
    // Bottom-right
    new Creatomate.Video({
      source: 'https://example.com/v4.mp4',
      x: '75%', y: '75%',
      width: '50%', height: '50%',
    }),
  ],
});
```

## Audio Element

### Background Music

```javascript
new Creatomate.Audio({
  source: 'https://example.com/music.mp3',

  // Match video length
  duration: null,

  // Volume
  volume: '50%',

  // Fading
  audioFadeIn: 1,   // 1 second fade in
  audioFadeOut: 2,  // 2 second fade out

  // Looping
  loop: true,
})
```

### Voiceover

```javascript
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  elements: [
    // Video with muted audio
    new Creatomate.Video({
      source: 'https://example.com/video.mp4',
      muted: true,  // Mute original audio
    }),

    // Voiceover track
    new Creatomate.Audio({
      source: 'https://example.com/voiceover.mp3',
      volume: '100%',
    }),

    // Background music (lower volume)
    new Creatomate.Audio({
      source: 'https://example.com/music.mp3',
      volume: '30%',
      audioFadeOut: 2,
    }),
  ],
});
```

## Transcoding

Convert video formats:

```javascript
// Transcode to MP4 H.264
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  elements: [
    new Creatomate.Video({
      source: 'https://example.com/input.mov',
    }),
  ],
});
```

## Video to GIF

```javascript
const source = new Creatomate.Source({
  outputFormat: 'gif',
  frameRate: 15,  // Lower for smaller file
  width: 480,
  height: 270,

  elements: [
    new Creatomate.Video({
      source: 'https://example.com/video.mp4',
      trimStart: 0,
      trimDuration: 5,  // 5 second GIF
    }),
  ],
});
```

## Screenshot / Snapshot

### Video Screenshot (Single Frame)

```javascript
const source = new Creatomate.Source({
  outputFormat: 'jpg',  // or 'png'
  width: 1920,
  height: 1080,

  elements: [
    new Creatomate.Video({
      source: 'https://example.com/video.mp4',
      time: 5,  // Capture at 5 seconds
    }),
  ],
});
```

### Snapshot Extraction

```javascript
// Extract thumbnail while rendering video
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  snapshotTime: 3.5,  // Extract frame at 3.5 seconds

  elements: [
    new Creatomate.Video({
      source: 'https://example.com/video.mp4',
    }),
  ],
});

const renders = await client.render({ source });
console.log('Video:', renders[0].url);
console.log('Thumbnail:', renders[0].snapshotUrl);
```

## Looping Video

```javascript
new Creatomate.Video({
  source: 'https://example.com/short-clip.mp4',
  loop: true,
  duration: 30,  // Loop for 30 seconds total
})
```

## See Also

- Concatenation example: `repo/concatenate/`
- Trim example: `repo/trim/`
- PiP example: `repo/picture-in-picture/`
- Split screen: `repo/splitscreen/`
- 2x2 wall: `repo/two-by-two/`
- 3x3 wall: `repo/three-by-three/`
- Transcode: `repo/transcode/`
- Video to GIF: `repo/video-to-gif/`
- Screenshot: `repo/video-screenshot/`
