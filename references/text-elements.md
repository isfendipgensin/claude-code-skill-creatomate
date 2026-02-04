---
name: text-elements
description: Creatomate text styling, fonts, backgrounds, shadows, and text animations
---

# Text Elements

Text elements display styled text with animations, backgrounds, and effects.

## Basic Text

```javascript
new Creatomate.Text({
  text: 'Hello World!',

  // Position
  x: '50%',
  y: '50%',
  width: '80%',
  height: '50%',

  // Alignment within bounds
  xAlignment: '50%',  // 0% = left, 50% = center, 100% = right
  yAlignment: '50%',  // 0% = top, 50% = center, 100% = bottom
})
```

## Font Styling

```javascript
new Creatomate.Text({
  text: 'Styled Text',

  // Font
  fontFamily: 'Montserrat',
  fontWeight: '700',       // '100' to '900', or 'bold'
  fontStyle: 'italic',     // 'normal', 'italic'
  fontSize: '8 vmin',      // Responsive size

  // Color
  fillColor: '#ffffff',

  // Stroke (outline)
  strokeColor: '#000000',
  strokeWidth: '1 vmin',

  // Line spacing
  lineHeight: '120%',
  letterSpacing: '0.05em',
})
```

## Using Font Objects

```javascript
new Creatomate.Text({
  text: 'Open Sans Bold',
  font: new Creatomate.Font('Open Sans', 700),
  fontSize: '48px',
})
```

## Text Background

Add a background box behind text:

```javascript
new Creatomate.Text({
  text: 'Text with background',
  background: new Creatomate.TextBackground(
    'rgba(255,255,255,0.8)',  // Background color
    '20%',                    // X padding
    '10%',                    // Y padding
    '5%',                     // Border radius X
    '5%'                      // Border radius Y
  ),
  fillColor: '#333333',
})
```

## Text Shadow

```javascript
new Creatomate.Text({
  text: 'Shadowed text',
  fillColor: '#ffffff',
  shadowColor: 'rgba(0,0,0,0.65)',
  shadowBlur: '1.6 vmin',
  shadowX: '2px',
  shadowY: '2px',
})
```

## TikTok-Style Text

Popular social media text style:

```javascript
new Creatomate.Text({
  text: 'TikTok style text!',

  fontFamily: 'Montserrat',
  fontWeight: '600',
  fontSize: '6 vmin',

  // White fill with black stroke
  fillColor: '#ffffff',
  strokeColor: '#000000',
  strokeWidth: '1 vmin',

  // Center alignment
  xAlignment: '50%',
  yAlignment: '50%',
})
```

## Padding

```javascript
new Creatomate.Text({
  text: 'Padded text',
  width: '100%',
  height: '100%',
  xPadding: '5 vw',   // Horizontal padding
  yPadding: '5 vh',   // Vertical padding
})
```

## Text Sizing Modes

```javascript
new Creatomate.Text({
  text: 'Auto-sized text',

  // Maximum font size (scales down to fit)
  fontSizeMaximum: '10 vmin',

  // Or fixed size
  fontSize: '48px',
})
```

## Rich Text with Colors

Use inline color tags:

```javascript
new Creatomate.Text({
  text: 'This is [color #f1c40f]highlighted[/color] text',
  fillColor: '#ffffff',
})
```

## Emojis

Set emoji style at source level:

```javascript
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  emojiStyle: 'apple',  // 'facebook', 'google', 'twitter', 'apple'
  elements: [
    new Creatomate.Text({
      text: 'Hello World! 🔥🎉'
    })
  ]
});
```

## Text Animations

### Slide Up Line by Line

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

### Slide Enter/Exit

```javascript
new Creatomate.Text({
  text: 'Animated text',
  enter: new Creatomate.TextSlide({
    duration: 2,
    easing: 'quadratic-out',
    split: 'line',
    scope: 'element',
    backgroundEffect: 'scaling-clip',
  }),
})
```

## Subtitles Container

Full-screen text container for subtitles:

```javascript
new Creatomate.Text({
  // Full screen with padding
  width: '100%',
  height: '100%',
  xPadding: '3 vmin',
  yPadding: '8 vmin',

  // Bottom center alignment
  xAlignment: '50%',
  yAlignment: '100%',

  // Style
  fontWeight: '800',
  fontSize: '8 vh',
  fillColor: null,  // Transparent fill
  shadowColor: 'rgba(0,0,0,0.65)',
  shadowBlur: '1.6 vmin',

  // Animated keyframes for subtitles
  text: subtitleKeyframes,  // Array of keyframe objects
})
```
