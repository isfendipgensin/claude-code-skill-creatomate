---
name: compositions
description: Creatomate compositions - grouping elements into scenes with nested timelines
---

# Compositions (Scenes)

Compositions group elements together, functioning as nested templates with their own timelines.

## Basic Composition

```javascript
new Creatomate.Composition({
  track: 1,
  duration: 5,

  elements: [
    // All elements in this array are grouped
    new Creatomate.Image({
      source: 'https://example.com/bg.jpg',
    }),
    new Creatomate.Text({
      text: 'Scene 1',
    }),
  ],
})
```

## Scene-Based Video

Create multi-scene videos by sequencing compositions:

```javascript
const source = new Creatomate.Source({
  outputFormat: 'mp4',
  width: 1080,
  height: 1920,
  frameRate: 60,

  elements: [
    // Background music (spans entire video)
    new Creatomate.Audio({
      source: 'https://example.com/music.mp3',
      duration: null,  // Match output length
      audioFadeOut: 2,
    }),

    // Scene 1
    new Creatomate.Composition({
      track: 1,
      duration: 5,
      elements: createScene1(),
    }),

    // Scene 2
    new Creatomate.Composition({
      track: 1,
      duration: 5,
      elements: createScene2(),
    }),

    // Scene 3
    new Creatomate.Composition({
      track: 1,
      duration: 5,
      elements: createScene3(),
    }),
  ],
});
```

## Scene Helper Functions

Organize scenes as separate functions:

```javascript
function createScene1() {
  return new Creatomate.Composition({
    track: 1,
    duration: 5,

    elements: [
      new Creatomate.Image({
        source: 'https://example.com/bg1.jpg',
        animations: [
          new Creatomate.Scale({
            easing: 'linear',
            startScale: '150%',
            endScale: '100%',
            fade: false,
          }),
        ],
      }),

      new Creatomate.Text({
        x: '50%',
        y: '33%',
        width: '88%',
        height: '40%',
        fontFamily: 'Montserrat',
        fontWeight: '600',
        fontSize: '6.2 vmin',
        xAlignment: '50%',
        yAlignment: '50%',
        fillColor: '#ffffff',
        strokeColor: '#000000',
        strokeWidth: '1 vmin',
        text: 'Scene 1 Title',
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
}
```

## Transitions Between Scenes

Add transitions between compositions:

```javascript
elements: [
  // Scene 1
  new Creatomate.Composition({
    track: 1,
    duration: 5,
    elements: [...],
  }),

  // Scene 2 with fade-in from Scene 1
  new Creatomate.Composition({
    track: 1,
    duration: 5,
    transition: new Creatomate.Fade({ duration: 1 }),
    elements: [...],
  }),
]
```

## Looping Compositions

Repeat a composition:

```javascript
new Creatomate.Composition({
  track: 1,
  loop: true,
  loopCount: 3,  // Repeat 3 times
  elements: [...],
})
```

## Composition Properties

```javascript
new Creatomate.Composition({
  // Sequencing
  track: 1,

  // Timing
  time: 0,       // Start time
  duration: 5,   // Duration

  // Size (optional, inherits from parent)
  width: '100%',
  height: '100%',

  // Position (optional)
  x: '50%',
  y: '50%',

  // Looping
  loop: false,
  loopCount: null,

  // Transition
  transition: null,

  // Content
  elements: [],
})
```

## Nested Compositions

Compositions can contain other compositions:

```javascript
new Creatomate.Composition({
  track: 1,
  duration: 10,
  elements: [
    // Background composition
    new Creatomate.Composition({
      elements: [...],
    }),

    // Overlay composition
    new Creatomate.Composition({
      elements: [...],
    }),
  ],
})
```

## Real-World Example: Story Video

```javascript
const Creatomate = require('creatomate');

const source = new Creatomate.Source({
  outputFormat: 'mp4',
  width: 1080,
  height: 1920,
  frameRate: 60,
  snapshotTime: 3.5,

  elements: [
    // Background music
    new Creatomate.Audio({
      source: 'https://example.com/music.mp3',
      duration: null,
      audioFadeOut: 2,
    }),

    // Scene 1: Title
    createTitleScene('Welcome to Our Product!', 'https://example.com/bg1.jpg'),

    // Scene 2: Feature 1
    createFeatureScene('Feature 1', 'Description here', 'https://example.com/bg2.jpg'),

    // Scene 3: Feature 2
    createFeatureScene('Feature 2', 'More info', 'https://example.com/bg3.jpg'),

    // Scene 4: CTA
    createCtaScene('Get Started Today!', 'https://example.com/bg4.jpg'),
  ],
});
```
