---
name: slideshows
description: Creatomate image slideshows with Ken Burns effects, transitions, and background music
---

# Slideshows

Create video slideshows from images with transitions and animations.

## Basic Slideshow

```javascript
const Creatomate = require('creatomate');
const client = new Creatomate.Client('YOUR_API_KEY');

const source = new Creatomate.Source({
  outputFormat: 'mp4',
  frameRate: 30,
  width: 1280,
  height: 720,

  elements: [
    // Image 1
    new Creatomate.Image({
      track: 1,
      duration: 5,
      source: 'https://example.com/image1.jpg',
    }),

    // Image 2
    new Creatomate.Image({
      track: 1,
      duration: 5,
      source: 'https://example.com/image2.jpg',
      transition: new Creatomate.Fade(),
    }),

    // Image 3
    new Creatomate.Image({
      track: 1,
      duration: 5,
      source: 'https://example.com/image3.jpg',
      transition: new Creatomate.Fade(),
    }),
  ],
});

const renders = await client.render({ source });
```

## Slideshow with Ken Burns Effect

Add subtle zoom/pan animations (Ken Burns effect):

```javascript
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  frameRate: 30,
  width: 1280,
  height: 720,

  elements: [
    // Image 1 - Zoom in
    new Creatomate.Image({
      track: 1,
      duration: 5,
      source: 'https://example.com/image1.jpg',
      animations: [
        new Creatomate.PanCenter({
          startScale: '100%',
          endScale: '120%',
          easing: 'linear',
        }),
      ],
    }),

    // Image 2 - Pan left with zoom
    new Creatomate.Image({
      track: 1,
      duration: 5,
      source: 'https://example.com/image2.jpg',
      animations: [
        new Creatomate.PanLeftWithZoom({
          startScale: '100%',
          endScale: '120%',
          easing: 'linear',
        }),
      ],
      transition: new Creatomate.Fade(),
    }),

    // Image 3 - Pan right with zoom
    new Creatomate.Image({
      track: 1,
      duration: 5,
      source: 'https://example.com/image3.jpg',
      animations: [
        new Creatomate.PanRightWithZoom({
          startScale: '100%',
          endScale: '120%',
          easing: 'linear',
        }),
      ],
      transition: new Creatomate.Fade(),
    }),
  ],
});
```

## Slideshow with Background Music

```javascript
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  frameRate: 30,
  width: 1280,
  height: 720,

  elements: [
    // Images on track 1
    new Creatomate.Image({
      track: 1,
      duration: 5,
      source: 'https://example.com/image1.jpg',
      animations: [new Creatomate.PanCenter({ startScale: '100%', endScale: '120%' })],
    }),
    new Creatomate.Image({
      track: 1,
      duration: 5,
      source: 'https://example.com/image2.jpg',
      animations: [new Creatomate.PanLeftWithZoom({ startScale: '100%', endScale: '120%' })],
      transition: new Creatomate.Fade(),
    }),
    new Creatomate.Image({
      track: 1,
      duration: 5,
      source: 'https://example.com/image3.jpg',
      animations: [new Creatomate.PanRightWithZoom({ startScale: '100%', endScale: '120%' })],
      transition: new Creatomate.Fade(),
    }),

    // Background music (spans entire video)
    new Creatomate.Audio({
      source: 'https://example.com/music.mp3',
      duration: null,  // Match video length
      audioFadeOut: 2, // Fade out at end
    }),
  ],
});
```

## Slideshow with Text Overlays

```javascript
const images = [
  { src: 'https://example.com/img1.jpg', caption: 'Beautiful Sunset' },
  { src: 'https://example.com/img2.jpg', caption: 'Mountain View' },
  { src: 'https://example.com/img3.jpg', caption: 'Ocean Waves' },
];

const elements = [];

images.forEach((img, index) => {
  // Image
  elements.push(new Creatomate.Image({
    track: 1,
    duration: 5,
    source: img.src,
    animations: [
      new Creatomate.PanCenter({
        startScale: '100%',
        endScale: '120%',
        easing: 'linear',
      }),
    ],
    transition: index > 0 ? new Creatomate.Fade() : undefined,
  }));

  // Caption text (on separate track to overlay)
  elements.push(new Creatomate.Text({
    track: 2,
    time: index * 5,  // Sync with image
    duration: 5,
    text: img.caption,
    y: '85%',
    width: '100%',
    xAlignment: '50%',
    fontFamily: 'Open Sans',
    fontWeight: '700',
    fontSize: '5 vmin',
    fillColor: '#ffffff',
    shadowColor: 'rgba(0,0,0,0.5)',
    shadowBlur: '10px',
  }));
});

const source = new Creatomate.Source({
  outputFormat: 'mp4',
  width: 1280,
  height: 720,
  elements,
});
```

## GIF Slideshow

```javascript
const source = new Creatomate.Source({
  outputFormat: 'gif',  // GIF output
  frameRate: 15,        // Lower frame rate for smaller file
  width: 600,
  height: 400,

  elements: [
    new Creatomate.Image({
      track: 1,
      duration: 2,
      source: 'https://example.com/image1.jpg',
    }),
    new Creatomate.Image({
      track: 1,
      duration: 2,
      source: 'https://example.com/image2.jpg',
      transition: new Creatomate.Fade({ duration: 0.5 }),
    }),
    new Creatomate.Image({
      track: 1,
      duration: 2,
      source: 'https://example.com/image3.jpg',
      transition: new Creatomate.Fade({ duration: 0.5 }),
    }),
  ],
});
```

## Dynamic Slideshow from Array

```javascript
function createSlideshow(imageUrls, durationPerSlide = 5) {
  const elements = imageUrls.map((url, index) => {
    const animations = [
      // Alternate between different Ken Burns effects
      index % 3 === 0
        ? new Creatomate.PanCenter({ startScale: '100%', endScale: '120%', easing: 'linear' })
        : index % 3 === 1
          ? new Creatomate.PanLeftWithZoom({ startScale: '100%', endScale: '120%', easing: 'linear' })
          : new Creatomate.PanRightWithZoom({ startScale: '100%', endScale: '120%', easing: 'linear' })
    ];

    return new Creatomate.Image({
      track: 1,
      duration: durationPerSlide,
      source: url,
      animations,
      transition: index > 0 ? new Creatomate.Fade() : undefined,
    });
  });

  return new Creatomate.Source({
    outputFormat: 'mp4',
    frameRate: 30,
    width: 1920,
    height: 1080,
    elements,
  });
}

// Usage
const source = createSlideshow([
  'https://example.com/photo1.jpg',
  'https://example.com/photo2.jpg',
  'https://example.com/photo3.jpg',
  'https://example.com/photo4.jpg',
]);
```
