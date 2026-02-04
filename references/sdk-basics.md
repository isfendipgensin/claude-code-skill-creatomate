---
name: sdk-basics
description: Creatomate SDK setup, API key configuration, and video/image rendering basics
---

# Creatomate SDK Basics

## Installation

```bash
npm install creatomate
```

## Client Setup

```javascript
const Creatomate = require('creatomate');

// API key from https://creatomate.com/docs/api/rest-api/authentication
const client = new Creatomate.Client('YOUR_API_KEY');
```

## Two Ways to Render

### 1. Using Templates (Recommended for Complex Designs)

Templates are created in the Creatomate Template Editor. You render them with modifications:

```javascript
const renders = await client.render({
  templateId: '2e8bccbf-e40a-41d5-a815-f58518ed9835',
  modifications: {
    'Title': 'Your headline here',
    'Text 1': 'Body text content',
    'Image-1.source': 'https://example.com/image.jpg'
  }
});
```

### 2. Using Source Objects (Programmatic)

Build video/image compositions entirely in code:

```javascript
const source = new Creatomate.Source({
  outputFormat: 'mp4',  // 'mp4', 'gif', 'jpg', 'png'
  width: 1920,
  height: 1080,
  frameRate: 30,        // Optional, default 30
  elements: [
    // Add elements here
  ]
});

const renders = await client.render({ source });
```

## Render Response

```javascript
const renders = await client.render({ source });

// renders is an array of render objects:
// [
//   {
//     id: 'render-id',
//     status: 'succeeded',
//     url: 'https://cdn.creatomate.com/renders/...',
//     snapshotUrl: 'https://...',  // If snapshotTime was set
//     // ... other metadata
//   }
// ]

console.log('Video URL:', renders[0].url);
```

## Source Object Properties

```javascript
const source = new Creatomate.Source({
  // Output format (required)
  outputFormat: 'mp4',  // 'mp4', 'gif', 'jpg', 'png'

  // Dimensions (auto-detected from first video if not set)
  width: 1920,
  height: 1080,

  // Frame rate (optional)
  frameRate: 30,

  // Emoji style (optional)
  emojiStyle: 'apple',  // 'facebook', 'google', 'twitter', 'apple'

  // Extract a thumbnail at this time (optional)
  snapshotTime: 3.5,

  // Content
  elements: []
});
```

## Common Aspect Ratios

| Format | Dimensions | Use Case |
|--------|-----------|----------|
| 16:9 Landscape | 1920x1080 | YouTube, standard video |
| 9:16 Portrait | 1080x1920 | TikTok, Instagram Stories, Reels |
| 1:1 Square | 1080x1080 | Instagram Feed, Facebook |
| 4:5 Portrait | 1080x1350 | Instagram Feed (optimal) |

## Error Handling

```javascript
try {
  const renders = await client.render({ source });
  console.log('Success:', renders[0].url);
} catch (error) {
  console.error('Render failed:', error.message);
}
```

## Async Rendering

For long videos, rendering is asynchronous. The `render()` method waits for completion by default. For webhook-based notifications:

```javascript
const renders = await client.render({
  source,
  webhookUrl: 'https://yourserver.com/webhook',
  metadata: 'your-custom-tracking-id'
});
```
