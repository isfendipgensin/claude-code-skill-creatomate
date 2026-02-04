---
name: images
description: Creatomate image elements, Ken Burns effects, watermarks, slideshows, and image masking
---

# Image Elements

Working with static images in Creatomate.

## Basic Image

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  duration: 5,  // Images need explicit duration for video output
})
```

## Image with Options

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',

  // Position
  x: '50%',
  y: '50%',
  xAnchor: '50%',
  yAnchor: '50%',

  // Size
  width: '100%',
  height: '100%',

  // Fit behavior
  fit: 'cover',  // 'cover' (crop), 'contain' (letterbox), 'fill' (stretch)

  // Timing
  time: 0,
  duration: 5,

  // Track for sequencing
  track: 1,
})
```

## Fit Modes

```javascript
// Cover: fills container, crops excess
new Creatomate.Image({
  source: 'https://example.com/wide-image.jpg',
  fit: 'cover',
  width: '100%',
  height: '100%',
})

// Contain: fits entirely, may have letterboxing
new Creatomate.Image({
  source: 'https://example.com/wide-image.jpg',
  fit: 'contain',
  width: '100%',
  height: '100%',
})

// Fill: stretches to fill (may distort)
new Creatomate.Image({
  source: 'https://example.com/wide-image.jpg',
  fit: 'fill',
  width: '100%',
  height: '100%',
})
```

## Ken Burns Effect

Add subtle zoom/pan animations to images:

### Zoom In (Center)

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  duration: 5,
  animations: [
    new Creatomate.PanCenter({
      startScale: '100%',
      endScale: '120%',
      easing: 'linear',
    }),
  ],
})
```

### Zoom Out

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  duration: 5,
  animations: [
    new Creatomate.Scale({
      startScale: '150%',
      endScale: '100%',
      easing: 'linear',
      fade: false,
    }),
  ],
})
```

### Pan Left with Zoom

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  duration: 5,
  animations: [
    new Creatomate.PanLeftWithZoom({
      startScale: '100%',
      endScale: '120%',
      easing: 'linear',
    }),
  ],
})
```

### Pan Right with Zoom

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  duration: 5,
  animations: [
    new Creatomate.PanRightWithZoom({
      startScale: '100%',
      endScale: '120%',
      easing: 'linear',
    }),
  ],
})
```

## Image Slideshow

```javascript
const images = [
  'https://example.com/image1.jpg',
  'https://example.com/image2.jpg',
  'https://example.com/image3.jpg',
];

const source = new Creatomate.Source({
  outputFormat: 'mp4',
  width: 1920,
  height: 1080,

  elements: images.map((src, i) => new Creatomate.Image({
    track: 1,
    duration: 5,
    source: src,
    animations: [
      new Creatomate.PanCenter({
        startScale: '100%',
        endScale: '120%',
        easing: 'linear',
      }),
    ],
    transition: i > 0 ? new Creatomate.Fade() : undefined,
  })),
});
```

## Watermark / Logo Overlay

```javascript
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  elements: [
    // Main video
    new Creatomate.Video({
      source: 'https://example.com/video.mp4',
    }),

    // Watermark (positioned in corner)
    new Creatomate.Image({
      source: 'https://example.com/logo.png',
      x: '95%',
      y: '5%',
      xAnchor: '100%',
      yAnchor: '0%',
      width: '15%',
      opacity: '70%',
    }),
  ],
});
```

## Responsive Watermark

Watermark that maintains aspect ratio:

```javascript
new Creatomate.Image({
  source: 'https://example.com/watermark.png',

  // Position at bottom-right with padding
  x: '100%',
  y: '100%',
  xAnchor: '100%',
  yAnchor: '100%',
  xPadding: '3%',
  yPadding: '3%',

  // Size relative to output
  width: '20%',

  // Semi-transparent
  opacity: '50%',
})
```

## Image Effects

### Blur

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  blur: '20px',
})
```

### Filters

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  brightness: '110%',
  contrast: '120%',
  saturation: '90%',
})
```

### Grayscale

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  filter: 'grayscale',
})
```

### Color Overlay

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  colorOverlay: '#ff0000',
  colorOverlayOpacity: '30%',
})
```

## Image Masking

Use a shape as a mask:

```javascript
new Creatomate.Image({
  source: 'https://example.com/photo.jpg',
  mask: new Creatomate.Ellipse({
    width: '80%',
    height: '80%',
  }),
})
```

## Warp (Perspective Transform)

Place images in perspective (e.g., on a phone screen mockup):

```javascript
new Creatomate.Image({
  source: 'https://example.com/screenshot.png',
  warp: {
    topLeft: { x: 10, y: 20 },
    topRight: { x: 90, y: 25 },
    bottomRight: { x: 85, y: 95 },
    bottomLeft: { x: 15, y: 90 },
  },
})
```

## Background Image with Video Overlay

```javascript
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  width: 1920,
  height: 1080,

  elements: [
    // Background image
    new Creatomate.Image({
      source: 'https://example.com/background.jpg',
      fit: 'cover',
    }),

    // Video in center
    new Creatomate.Video({
      source: 'https://example.com/video.mp4',
      width: '60%',
      height: '60%',
      borderRadius: '10px',
      shadow: {
        color: 'rgba(0,0,0,0.5)',
        blur: '20px',
      },
    }),
  ],
});
```

## Static Image Output

Generate JPG/PNG images:

```javascript
// Single image output
const source = new Creatomate.Source({
  outputFormat: 'jpg',  // or 'png' for transparency
  width: 1200,
  height: 630,  // Social media preview size

  elements: [
    new Creatomate.Image({
      source: 'https://example.com/background.jpg',
    }),
    new Creatomate.Text({
      text: 'Your Title Here',
      y: '50%',
      width: '90%',
      xAlignment: '50%',
      fontFamily: 'Montserrat',
      fontWeight: '700',
      fontSize: '8 vmin',
      fillColor: '#ffffff',
    }),
  ],
});
```

## See Also

- Slideshow example: `repo/slideshow/`
- Watermark example: `repo/watermark/`
- Responsive overlay: `repo/responsive-overlay/`
- Warp (mockups): `repo/warp-image/`
- Blur background: `repo/blur-background/`
- Filters: `repo/filters/`
- Mask: `repo/mask/`
