---
name: effects
description: Creatomate visual effects - blur, filters, masks, shadows, color overlays, and blend modes
---

# Effects

Creatomate supports various visual effects for elements.

## Opacity

Make elements partially transparent:

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  opacity: '50%',  // 0% = invisible, 100% = fully opaque
})
```

## Rotation

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  rotation: 45,  // Degrees
})
```

## Shadow

Add drop shadows to elements:

```javascript
new Creatomate.Text({
  text: 'Shadowed',
  shadowColor: 'rgba(0,0,0,0.5)',
  shadowBlur: '10px',
  shadowX: '5px',
  shadowY: '5px',
})
```

## Color Overlay

Apply a color tint to elements:

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  colorOverlay: '#ff0000',
  colorOverlayOpacity: '30%',
})
```

## Filters

Apply color filters to elements:

```javascript
new Creatomate.Video({
  source: 'https://example.com/video.mp4',

  // Individual filters
  brightness: '120%',   // Default 100%
  contrast: '110%',     // Default 100%
  saturation: '80%',    // Default 100%
  hue: '180',           // Degrees (0-360)
  blur: '5px',          // Blur amount

  // Or use preset filter
  filter: 'grayscale',  // Makes element black & white
})
```

## Blur Background

Create a blurred background effect:

```javascript
elements: [
  // Blurred background layer
  new Creatomate.Video({
    source: 'https://example.com/video.mp4',
    blur: '20px',
  }),

  // Sharp foreground
  new Creatomate.Video({
    source: 'https://example.com/video.mp4',
    width: '60%',
    height: '60%',
  }),
]
```

## Blend Modes

Blend elements with layers below:

```javascript
new Creatomate.Image({
  source: 'https://example.com/overlay.png',
  blendMode: 'multiply',
  // Options: 'normal', 'multiply', 'screen', 'overlay',
  //          'darken', 'lighten', 'color-dodge', 'color-burn',
  //          'hard-light', 'soft-light', 'difference', 'exclusion'
})
```

## Masks

Use one element as a mask for another:

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  mask: new Creatomate.Ellipse({
    width: '50%',
    height: '50%',
  }),
})
```

## Repeat (Pattern Fill)

Use an element as a repeating pattern:

```javascript
new Creatomate.Image({
  source: 'https://example.com/pattern.png',
  repeat: true,
  repeatWidth: '100px',
  repeatHeight: '100px',
})
```

## Border Radius

Round corners on rectangular elements:

```javascript
new Creatomate.Rectangle({
  width: '200px',
  height: '100px',
  fillColor: '#ff0000',
  borderRadius: '20px',  // Or percentage like '50%'
})
```

## Warp (Perspective Distortion)

Apply perspective transformations for mockups:

```javascript
new Creatomate.Image({
  source: 'https://example.com/screenshot.png',
  warp: {
    // Four corner coordinates
    topLeft: { x: 10, y: 20 },
    topRight: { x: 90, y: 25 },
    bottomRight: { x: 85, y: 95 },
    bottomLeft: { x: 15, y: 90 },
  },
})
```

## Progress Bar Example

Animated progress bar using wipe animation:

```javascript
new Creatomate.Rectangle({
  x: '0%',
  y: '0%',
  width: '100%',
  height: '3%',
  xAnchor: '0%',
  yAnchor: '0%',
  fillColor: '#ffffff',
  animations: [
    new Creatomate.Wipe({
      xAnchor: '0%',
      fade: false,
      easing: 'linear',
    }),
  ],
})
```
