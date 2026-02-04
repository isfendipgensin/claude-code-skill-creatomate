---
name: source-elements
description: Creatomate element types (Video, Image, Audio, Text), positioning, tracks, and layering
---

# Source Elements

Elements are the building blocks of Creatomate compositions. Each element represents a visual or audio component.

## Element Types

| Type | Class | Description |
|------|-------|-------------|
| Video | `Creatomate.Video` | Video files |
| Image | `Creatomate.Image` | Static images |
| Audio | `Creatomate.Audio` | Audio tracks |
| Text | `Creatomate.Text` | Text overlays |
| Rectangle | `Creatomate.Rectangle` | Solid rectangles |
| Ellipse | `Creatomate.Ellipse` | Circles/ellipses |
| Composition | `Creatomate.Composition` | Grouped elements (scenes) |

## Common Element Properties

All elements share these positioning and timing properties:

```javascript
new Creatomate.Video({
  // Source
  source: 'https://example.com/video.mp4',

  // Position (default: centered)
  x: '50%',      // Horizontal position
  y: '50%',      // Vertical position
  xAnchor: '50%', // Anchor point X
  yAnchor: '50%', // Anchor point Y

  // Size
  width: '100%',
  height: '100%',

  // Timing
  time: 0,       // Start time in seconds
  duration: 5,   // Duration (null = auto from media)

  // Track (for sequencing)
  track: 1,      // Elements on same track play sequentially
})
```

## Units

Creatomate supports multiple unit types:

| Unit | Example | Description |
|------|---------|-------------|
| Pixels | `100` | Absolute pixels |
| Percent | `'50%'` | Percentage of output size |
| vw | `'10 vw'` | Percentage of width |
| vh | `'10 vh'` | Percentage of height |
| vmin | `'5 vmin'` | Percentage of smaller dimension |
| vmax | `'5 vmax'` | Percentage of larger dimension |

## Tracks (Sequential Elements)

Elements on the same track number play sequentially:

```javascript
elements: [
  // These play one after another (same track)
  new Creatomate.Video({
    track: 1,
    source: 'https://example.com/video1.mp4'
  }),
  new Creatomate.Video({
    track: 1,
    source: 'https://example.com/video2.mp4'
  }),

  // This plays on a separate track (overlaps)
  new Creatomate.Text({
    track: 2,
    text: 'Overlaid text'
  })
]
```

## Video Element

```javascript
new Creatomate.Video({
  source: 'https://example.com/video.mp4',

  // Trimming
  trimStart: 2,      // Start at 2 seconds
  trimDuration: 10,  // Use 10 seconds

  // Audio
  volume: '80%',
  audioFadeIn: 1,    // Fade in duration
  audioFadeOut: 2,   // Fade out duration
  muted: false,

  // Looping
  loop: true,

  // Playback
  playbackRate: 1.0, // Speed multiplier
})
```

## Image Element

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  duration: 5,  // Images need explicit duration

  // Fit behavior
  fit: 'cover',  // 'cover', 'contain', 'fill'
})
```

## Audio Element

```javascript
new Creatomate.Audio({
  source: 'https://example.com/music.mp3',

  // Duration (null = match output length)
  duration: null,

  // Volume and fading
  volume: '50%',
  audioFadeIn: 1,
  audioFadeOut: 2,

  // Looping
  loop: true,
})
```

## Rectangle Element

```javascript
new Creatomate.Rectangle({
  x: '0%',
  y: '0%',
  width: '100%',
  height: '5%',
  xAnchor: '0%',
  yAnchor: '0%',
  fillColor: '#ff0000',
  borderRadius: '10px',
})
```

## Layering (Z-Order)

Elements are layered in array order (last element is on top):

```javascript
elements: [
  new Creatomate.Video({ source: '...' }),     // Bottom layer
  new Creatomate.Rectangle({ ... }),            // Middle layer
  new Creatomate.Text({ text: 'On top' }),     // Top layer
]
```
