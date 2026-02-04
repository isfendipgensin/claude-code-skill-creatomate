---
name: animations
description: Creatomate animations - keyframes, transitions, easing functions, and enter/exit effects
---

# Animations

Creatomate provides extensive animation capabilities for elements.

## Animation Types

### Scale Animation

Zoom in/out effect:

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  animations: [
    new Creatomate.Scale({
      easing: 'linear',
      startScale: '100%',
      endScale: '120%',
      fade: false,
    }),
  ],
})
```

### Pan Animations (Ken Burns Effect)

```javascript
// Pan with zoom
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  animations: [
    new Creatomate.PanCenter({
      startScale: '100%',
      endScale: '120%',
      easing: 'linear',
    }),
  ],
})

// Pan left with zoom
new Creatomate.PanLeftWithZoom({
  startScale: '100%',
  endScale: '120%',
  easing: 'linear',
})

// Pan right with zoom
new Creatomate.PanRightWithZoom({
  startScale: '100%',
  endScale: '120%',
  easing: 'linear',
})
```

### Wipe Animation

Progress bar / reveal effect:

```javascript
new Creatomate.Wipe({
  xAnchor: '0%',    // Start from left
  fade: false,
  easing: 'linear',
})
```

## Transitions

Transitions play between sequential elements on the same track:

### Fade Transition

```javascript
new Creatomate.Image({
  track: 1,
  source: 'https://example.com/image2.jpg',
  transition: new Creatomate.Fade({
    duration: 1,  // 1 second crossfade
  }),
})
```

### Available Transitions

```javascript
// Fade
new Creatomate.Fade({ duration: 1 })

// Slide
new Creatomate.Slide({
  duration: 1,
  direction: 'left',  // 'left', 'right', 'up', 'down'
})

// Wipe
new Creatomate.WipeTransition({
  duration: 1,
  direction: 'left',
})

// Zoom
new Creatomate.Zoom({
  duration: 1,
})
```

## Text Animations

### Text Slide

```javascript
new Creatomate.Text({
  text: 'Animated text',
  enter: new Creatomate.TextSlide({
    duration: 2,
    easing: 'quadratic-out',
    split: 'line',           // 'line', 'word', 'character'
    scope: 'element',
    backgroundEffect: 'scaling-clip',
  }),
})
```

### Text Slide Up Line by Line

```javascript
new Creatomate.Text({
  text: 'Line 1\nLine 2\nLine 3',
  animations: [
    new Creatomate.TextSlideUpLineByLine({
      time: 0,
      duration: 1.5,
      easing: 'quadratic-out',
      scope: 'split-clip',
      distance: '100%',
    }),
  ],
})
```

## Keyframes

Animate any property over time:

```javascript
new Creatomate.Image({
  source: 'https://example.com/image.jpg',
  opacity: [
    { time: 0, value: '0%' },
    { time: 1, value: '100%' },
    { time: 4, value: '100%' },
    { time: 5, value: '0%' },
  ],
})
```

### Keyframe with Easing

```javascript
new Creatomate.Text({
  text: 'Moving text',
  x: [
    { time: 0, value: '0%', easing: 'quadratic-out' },
    { time: 2, value: '100%' },
  ],
})
```

## Easing Functions

Available easing options:

| Easing | Description |
|--------|-------------|
| `linear` | Constant speed |
| `quadratic-in` | Slow start |
| `quadratic-out` | Slow end |
| `quadratic-in-out` | Slow start and end |
| `cubic-in` | Slower start |
| `cubic-out` | Slower end |
| `cubic-in-out` | Slower start and end |
| `back-in` | Overshoot at start |
| `back-out` | Overshoot at end |
| `back-in-out` | Overshoot both |
| `elastic-in` | Elastic start |
| `elastic-out` | Elastic end |
| `bounce-out` | Bouncing effect |

## Stroke Animation

Animate drawing of shape outlines:

```javascript
new Creatomate.Shape({
  path: 'M 0 0 L 100 0 L 100 100 L 0 100 Z',
  strokeColor: '#ffffff',
  strokeWidth: '3px',
  fillColor: null,
  animations: [
    new Creatomate.StrokeDraw({
      duration: 2,
      easing: 'quadratic-out',
    }),
  ],
})
```

## Timing Properties

Control when animations play:

```javascript
new Creatomate.Scale({
  time: 2,        // Start at 2 seconds
  duration: 3,    // Play for 3 seconds
  easing: 'quadratic-out',
  // ...
})
```
