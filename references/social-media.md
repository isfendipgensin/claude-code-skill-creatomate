---
name: social-media
description: Creatomate video formats for TikTok, Instagram Reels/Stories, YouTube Shorts
---

# Social Media Formats

Create optimized videos for TikTok, Instagram, YouTube Shorts, and other platforms.

## Aspect Ratios

| Platform | Format | Dimensions | Aspect Ratio |
|----------|--------|------------|--------------|
| TikTok | Vertical | 1080x1920 | 9:16 |
| Instagram Stories | Vertical | 1080x1920 | 9:16 |
| Instagram Reels | Vertical | 1080x1920 | 9:16 |
| YouTube Shorts | Vertical | 1080x1920 | 9:16 |
| Instagram Feed | Square | 1080x1080 | 1:1 |
| Instagram Feed | Portrait | 1080x1350 | 4:5 |
| YouTube | Landscape | 1920x1080 | 16:9 |
| Facebook | Landscape | 1920x1080 | 16:9 |
| Twitter/X | Landscape | 1280x720 | 16:9 |

## TikTok / Instagram Reels / YouTube Shorts

```javascript
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  width: 1080,
  height: 1920,  // 9:16 vertical
  frameRate: 60,

  elements: [
    // Background
    new Creatomate.Image({
      source: 'https://example.com/background.jpg',
      animations: [
        new Creatomate.Scale({
          easing: 'linear',
          startScale: '150%',
          endScale: '100%',
          fade: false,
        }),
      ],
    }),

    // TikTok-style text
    new Creatomate.Text({
      x: '50%',
      y: '40%',
      width: '88%',
      height: '40%',

      fontFamily: 'Montserrat',
      fontWeight: '600',
      fontSize: '6 vmin',
      lineHeight: '100%',

      xAlignment: '50%',
      yAlignment: '50%',

      // TikTok default style
      fillColor: '#ffffff',
      strokeColor: '#000000',
      strokeWidth: '1 vmin',

      text: 'Your TikTok Text Here! 🔥',

      animations: [
        new Creatomate.TextSlideUpLineByLine({
          time: 0,
          duration: 1.5,
          easing: 'quadratic-out',
          scope: 'split-clip',
          distance: '100%',
        }),
      ],
    }),
  ],
});
```

## Story Video with Multiple Scenes

```javascript
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  width: 1080,
  height: 1920,
  frameRate: 60,
  snapshotTime: 3.5,  // Thumbnail extraction

  elements: [
    // Background music
    new Creatomate.Audio({
      source: 'https://example.com/upbeat-music.mp3',
      duration: null,
      audioFadeOut: 2,
    }),

    // Scene 1: Hook
    createStoryScene({
      duration: 3,
      background: 'https://example.com/scene1.jpg',
      text: 'Did you know? 🤔',
      position: 'center',
    }),

    // Scene 2: Point 1
    createStoryScene({
      duration: 4,
      background: 'https://example.com/scene2.jpg',
      text: 'First amazing fact about this topic',
      position: 'top',
    }),

    // Scene 3: Point 2
    createStoryScene({
      duration: 4,
      background: 'https://example.com/scene3.jpg',
      text: 'Second mind-blowing detail',
      position: 'bottom',
    }),

    // Scene 4: CTA
    createStoryScene({
      duration: 3,
      background: 'https://example.com/scene4.jpg',
      text: 'Follow for more! 🙌',
      position: 'center',
    }),
  ],
});

function createStoryScene({ duration, background, text, position }) {
  const yPositions = {
    top: '25%',
    center: '50%',
    bottom: '75%',
  };

  return new Creatomate.Composition({
    track: 1,
    duration,

    elements: [
      new Creatomate.Image({
        source: background,
        animations: [
          new Creatomate.Scale({
            easing: 'linear',
            startScale: '120%',
            endScale: '100%',
            fade: false,
          }),
        ],
      }),

      new Creatomate.Text({
        x: '50%',
        y: yPositions[position],
        width: '85%',
        xAlignment: '50%',
        yAlignment: '50%',

        fontFamily: 'Montserrat',
        fontWeight: '700',
        fontSize: '7 vmin',

        fillColor: '#ffffff',
        strokeColor: '#000000',
        strokeWidth: '0.8 vmin',

        text,

        animations: [
          new Creatomate.TextSlideUpLineByLine({
            duration: 0.8,
            easing: 'quadratic-out',
            scope: 'split-clip',
            distance: '50%',
          }),
        ],
      }),
    ],
  });
}
```

## Instagram Feed (Square)

```javascript
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  width: 1080,
  height: 1080,  // 1:1 square
  frameRate: 30,

  elements: [
    new Creatomate.Video({
      source: 'https://example.com/video.mp4',
      fit: 'cover',
    }),

    new Creatomate.Text({
      y: '85%',
      width: '90%',
      xAlignment: '50%',

      fontFamily: 'Inter',
      fontWeight: '600',
      fontSize: '5 vmin',
      fillColor: '#ffffff',
      shadowColor: 'rgba(0,0,0,0.5)',
      shadowBlur: '8px',

      text: 'Caption text here',
    }),
  ],
});
```

## YouTube Landscape

```javascript
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  width: 1920,
  height: 1080,  // 16:9 landscape
  frameRate: 30,

  elements: [
    new Creatomate.Video({
      source: 'https://example.com/main-video.mp4',
    }),

    // Lower third title
    new Creatomate.Rectangle({
      x: '0%',
      y: '85%',
      width: '40%',
      height: '15%',
      xAnchor: '0%',
      fillColor: 'rgba(0,0,0,0.7)',
    }),

    new Creatomate.Text({
      x: '2%',
      y: '88%',
      width: '36%',
      xAnchor: '0%',
      xAlignment: '0%',

      fontFamily: 'Roboto',
      fontWeight: '700',
      fontSize: '3 vh',
      fillColor: '#ffffff',

      text: 'John Smith\nProduct Manager',
    }),
  ],
});
```

## Multi-Format Export

Generate multiple formats from the same content:

```javascript
async function exportAllFormats(content) {
  const client = new Creatomate.Client(apiKey);

  const formats = [
    { name: 'tiktok', width: 1080, height: 1920 },
    { name: 'instagram-feed', width: 1080, height: 1080 },
    { name: 'youtube', width: 1920, height: 1080 },
  ];

  const results = {};

  for (const format of formats) {
    const source = new Creatomate.Source({
      outputFormat: 'mp4',
      width: format.width,
      height: format.height,
      elements: content.elements,
    });

    const renders = await client.render({ source });
    results[format.name] = renders[0].url;
  }

  return results;
}
```

## Platform-Specific Tips

### TikTok
- Use bold, high-contrast text
- Keep videos 15-60 seconds
- Add trending music
- Use 60fps for smooth motion

### Instagram Stories/Reels
- Include text for silent viewing
- Use Instagram-native fonts (Montserrat, Open Sans)
- Add stickers/emojis

### YouTube Shorts
- Hook viewers in first 3 seconds
- Use clear, readable text
- Include call-to-action
- Optimal length: 30-60 seconds
