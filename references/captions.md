---
name: captions
description: Creatomate animated captions and subtitles with AWS Transcribe integration
---

# Captions & Subtitles

Add animated captions and subtitles to videos using AWS Transcribe or other transcription services.

## Caption Workflow

1. Transcribe video audio using AWS Transcribe (or similar)
2. Convert transcription to Creatomate keyframes
3. Render video with animated captions

## Basic Caption Setup

```javascript
const Creatomate = require('creatomate');

const source = new Creatomate.Source({
  outputFormat: 'mp4',

  elements: [
    // Video
    new Creatomate.Video({
      source: 'https://example.com/video.mp4',
    }),

    // Captions overlay
    new Creatomate.Text({
      width: '100%',
      height: '100%',
      xPadding: '3 vmin',
      yPadding: '8 vmin',

      // Bottom center alignment
      xAlignment: '50%',
      yAlignment: '100%',

      // Caption styling
      fontWeight: '800',
      fontSize: '8 vh',
      fillColor: null,  // Transparent fill for glow effect
      shadowColor: 'rgba(0,0,0,0.65)',
      shadowBlur: '1.6 vmin',

      // Animated subtitles (keyframes)
      text: captionKeyframes,
    }),
  ],
});
```

## Caption Keyframes Format

Captions use keyframes to change text at specific times:

```javascript
const captionKeyframes = [
  { time: 0, value: '' },
  { time: 0.5, value: 'Hello and welcome' },
  { time: 2.0, value: 'to this video' },
  { time: 3.5, value: 'about Creatomate' },
  { time: 5.0, value: '' },
];
```

## AWS Transcribe Integration

### Step 1: Transcribe with AWS

```javascript
const { TranscribeClient, StartTranscriptionJobCommand, GetTranscriptionJobCommand } = require('@aws-sdk/client-transcribe');

async function transcribeVideo(mediaUri, awsRegion, bucketName, bucketKey, jobName) {
  const client = new TranscribeClient({ region: awsRegion });

  // Start transcription job
  await client.send(new StartTranscriptionJobCommand({
    TranscriptionJobName: jobName,
    LanguageCode: 'en-US',
    MediaFormat: 'mp4',
    Media: { MediaFileUri: mediaUri },
    OutputBucketName: bucketName,
    OutputKey: bucketKey,
  }));

  // Wait for completion
  let status = 'IN_PROGRESS';
  while (status === 'IN_PROGRESS') {
    await new Promise(resolve => setTimeout(resolve, 5000));
    const response = await client.send(new GetTranscriptionJobCommand({
      TranscriptionJobName: jobName,
    }));
    status = response.TranscriptionJob.TranscriptionJobStatus;
  }
}
```

### Step 2: Convert Transcription to Keyframes

```javascript
const { S3Client, GetObjectCommand } = require('@aws-sdk/client-s3');

async function generateSubtitles(awsRegion, bucketName, bucketKey) {
  const s3 = new S3Client({ region: awsRegion });
  const response = await s3.send(new GetObjectCommand({
    Bucket: bucketName,
    Key: bucketKey + '.json',
  }));

  const transcription = JSON.parse(await response.Body.transformToString());
  const items = transcription.results.items;

  const keyframes = [];
  let currentText = '';
  let wordCount = 0;

  for (const item of items) {
    if (item.type === 'pronunciation') {
      const time = parseFloat(item.start_time);
      const word = item.alternatives[0].content;

      currentText += (wordCount > 0 ? ' ' : '') + word;
      wordCount++;

      // Create new keyframe every 5 words
      if (wordCount >= 5) {
        keyframes.push({ time, value: currentText });
        currentText = '';
        wordCount = 0;
      }
    } else if (item.type === 'punctuation') {
      currentText += item.alternatives[0].content;
    }
  }

  // Add remaining text
  if (currentText) {
    keyframes.push({
      time: parseFloat(items[items.length - 1].end_time),
      value: currentText
    });
  }

  return keyframes;
}
```

### Step 3: Render with Captions

```javascript
async function createCaptionedVideo(videoUrl, captionKeyframes) {
  const client = new Creatomate.Client('YOUR_API_KEY');

  const source = new Creatomate.Source({
    outputFormat: 'mp4',

    elements: [
      new Creatomate.Video({
        source: videoUrl,
      }),

      new Creatomate.Text({
        width: '100%',
        height: '100%',
        xPadding: '3 vmin',
        yPadding: '8 vmin',
        xAlignment: '50%',
        yAlignment: '100%',
        fontWeight: '800',
        fontSize: '8 vh',
        fillColor: null,
        shadowColor: 'rgba(0,0,0,0.65)',
        shadowBlur: '1.6 vmin',
        text: captionKeyframes,
      }),

      // Progress bar
      new Creatomate.Rectangle({
        x: '0%',
        y: '0%',
        width: '100%',
        height: '3%',
        xAnchor: '0%',
        yAnchor: '0%',
        fillColor: '#fff',
        animations: [
          new Creatomate.Wipe({
            xAnchor: '0%',
            fade: false,
            easing: 'linear',
          }),
        ],
      }),
    ],
  });

  return await client.render({ source });
}
```

## Caption Styling Options

### Standard Subtitles (White with Shadow)

```javascript
new Creatomate.Text({
  fontFamily: 'Arial',
  fontWeight: '700',
  fontSize: '6 vh',
  fillColor: '#ffffff',
  shadowColor: 'rgba(0,0,0,0.8)',
  shadowBlur: '4px',
})
```

### Netflix-Style Captions

```javascript
new Creatomate.Text({
  fontFamily: 'Arial',
  fontWeight: '400',
  fontSize: '5 vh',
  fillColor: '#ffffff',
  background: new Creatomate.TextBackground(
    'rgba(0,0,0,0.75)',
    '10px',
    '5px',
    '3px',
    '3px'
  ),
})
```

### TikTok-Style Animated Captions

```javascript
new Creatomate.Text({
  fontFamily: 'Montserrat',
  fontWeight: '800',
  fontSize: '8 vmin',
  fillColor: '#ffffff',
  strokeColor: '#000000',
  strokeWidth: '0.5 vmin',
  enter: new Creatomate.TextSlide({
    duration: 0.3,
    easing: 'quadratic-out',
    split: 'word',
  }),
})
```

### Highlighted Word Effect

Use inline color tags for emphasis:

```javascript
const captionKeyframes = [
  { time: 0, value: 'This is [color #ffff00]important[/color] information' },
  { time: 2, value: 'Pay [color #ff0000]attention[/color] now' },
];
```

## Full AWS Transcribe Example

See the complete example at `repo/captions/index.js` and related files:
- `repo/captions/transcribe.js` - AWS Transcribe integration
- `repo/captions/generateSubtitles.js` - Keyframe generation

```javascript
// Full example flow
const Creatomate = require('creatomate');
const transcribe = require('./transcribe');
const generateSubtitles = require('./generateSubtitles');

async function run() {
  const mediaUri = 'https://example.com/video.mp4';

  // Transcribe video
  await transcribe(jobName, mediaUri, awsRegion, bucketName, bucketKey);

  // Generate subtitle keyframes
  const subtitleKeyframes = await generateSubtitles(awsRegion, bucketName, bucketKey);

  // Create video with captions
  const client = new Creatomate.Client(apiKey);
  const renders = await client.render({
    source: new Creatomate.Source({
      outputFormat: 'mp4',
      elements: [
        new Creatomate.Video({ source: mediaUri }),
        new Creatomate.Text({
          /* caption styling */
          text: subtitleKeyframes,
        }),
      ],
    }),
  });

  console.log('Captioned video:', renders[0].url);
}
```
