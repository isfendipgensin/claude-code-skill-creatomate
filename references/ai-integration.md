---
name: ai-integration
description: Creatomate integration with ChatGPT, DALL-E, AWS Polly for AI-generated video content
---

# AI Integration

Integrate Creatomate with AI services like ChatGPT to generate dynamic content.

## ChatGPT Integration

Generate video content using OpenAI's ChatGPT:

```javascript
const Creatomate = require('creatomate');
const fetch = require('node-fetch');

const creatomateApiKey = 'YOUR_CREATOMATE_KEY';
const openAiApiKey = 'YOUR_OPENAI_KEY';
const templateId = 'YOUR_TEMPLATE_ID';

async function generateAIVideo(topic) {
  // Step 1: Get content from ChatGPT
  const response = await fetch('https://api.openai.com/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${openAiApiKey}`,
    },
    body: JSON.stringify({
      model: 'gpt-4',
      messages: [{
        role: 'user',
        content: `${topic}. Make a numbered list of 5 short sentences with emojis. ` +
          `Surround the most important words with [color #f1c40f] and [/color].`
      }],
    }),
  });

  const data = await response.json();
  const content = data.choices[0].message.content;

  // Step 2: Parse the response
  const facts = {};
  content.split(/\r?\n/).forEach(line => {
    const match = line.match(/^(\d+)/);
    if (match) {
      facts[`Fact-${match[1]}`] = line;
    }
  });

  // Step 3: Render video with AI content
  const client = new Creatomate.Client(creatomateApiKey);

  const renders = await client.render({
    templateId,
    modifications: {
      'Intro-Text': topic,
      ...facts,
    },
  });

  return renders[0].url;
}

// Usage
const videoUrl = await generateAIVideo('5 fascinating facts about the Golden Eagle');
```

## Prompt Engineering for Video Content

### Structured List Content

```javascript
const prompt = `
Create 5 facts about ${topic}.
Format each fact as a numbered list.
Each fact should be one sentence, under 15 words.
Include relevant emojis.
Highlight key words with [color #ffcc00] and [/color].
`;
```

### Script Generation

```javascript
const prompt = `
Write a 30-second script about ${topic}.
Structure:
- Hook (5 seconds): Attention-grabbing opening
- Main points (20 seconds): 3 key facts
- CTA (5 seconds): Call to action

Use conversational tone.
Add [PAUSE] markers for timing.
`;
```

### Social Media Captions

```javascript
const prompt = `
Write 5 Instagram caption options for a video about ${topic}.
Each caption should:
- Start with a hook or question
- Be 150-200 characters
- Include 3-5 relevant hashtags
- End with a call-to-action
`;
```

## Building Videos from AI Content

### Dynamic Scene Generation

```javascript
async function createAIGeneratedVideo(topic) {
  // Generate script with ChatGPT
  const script = await generateScript(topic);

  // Parse script into scenes
  const scenes = parseScriptToScenes(script);

  // Build video
  const elements = [
    // Background music
    new Creatomate.Audio({
      source: 'https://example.com/background-music.mp3',
      duration: null,
      audioFadeOut: 2,
    }),
  ];

  scenes.forEach((scene, index) => {
    elements.push(new Creatomate.Composition({
      track: 1,
      duration: scene.duration,
      elements: [
        new Creatomate.Image({
          source: scene.backgroundUrl,
          animations: [
            new Creatomate.PanCenter({
              startScale: '100%',
              endScale: '120%',
            }),
          ],
        }),
        new Creatomate.Text({
          y: '50%',
          width: '85%',
          xAlignment: '50%',
          yAlignment: '50%',
          fontFamily: 'Montserrat',
          fontWeight: '700',
          fontSize: '6 vmin',
          fillColor: '#ffffff',
          strokeColor: '#000000',
          strokeWidth: '0.8 vmin',
          text: scene.text,
        }),
      ],
    }));
  });

  const source = new Creatomate.Source({
    outputFormat: 'mp4',
    width: 1080,
    height: 1920,
    elements,
  });

  const client = new Creatomate.Client(apiKey);
  return await client.render({ source });
}
```

## DALL-E / Stable Diffusion Integration

Generate background images with AI:

```javascript
async function generateBackgroundWithDALLE(prompt) {
  const response = await fetch('https://api.openai.com/v1/images/generations', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${openAiApiKey}`,
    },
    body: JSON.stringify({
      model: 'dall-e-3',
      prompt: `${prompt}, cinematic lighting, high quality, suitable as video background`,
      n: 1,
      size: '1792x1024',
    }),
  });

  const data = await response.json();
  return data.data[0].url;
}

// Use in video creation
async function createVideoWithAIBackground(topic) {
  const backgroundUrl = await generateBackgroundWithDALLE(
    `Abstract background representing ${topic}`
  );

  const source = new Creatomate.Source({
    outputFormat: 'mp4',
    width: 1920,
    height: 1080,
    elements: [
      new Creatomate.Image({
        source: backgroundUrl,
        duration: 10,
      }),
      // ... other elements
    ],
  });

  const client = new Creatomate.Client(apiKey);
  return await client.render({ source });
}
```

## Text-to-Speech Integration

### AWS Polly

```javascript
const { PollyClient, SynthesizeSpeechCommand } = require('@aws-sdk/client-polly');
const { S3Client, PutObjectCommand } = require('@aws-sdk/client-s3');

async function generateSpeech(text, voiceId = 'Matthew') {
  const polly = new PollyClient({ region: 'us-east-1' });

  const response = await polly.send(new SynthesizeSpeechCommand({
    Text: text,
    OutputFormat: 'mp3',
    VoiceId: voiceId,
    Engine: 'neural',
  }));

  // Upload to S3 for use in Creatomate
  const s3 = new S3Client({ region: 'us-east-1' });
  const key = `audio/speech-${Date.now()}.mp3`;

  await s3.send(new PutObjectCommand({
    Bucket: 'your-bucket',
    Key: key,
    Body: response.AudioStream,
    ContentType: 'audio/mpeg',
  }));

  return `https://your-bucket.s3.amazonaws.com/${key}`;
}

// Use in video
const audioUrl = await generateSpeech('Hello, welcome to our video!');

const source = new Creatomate.Source({
  outputFormat: 'mp4',
  elements: [
    new Creatomate.Video({ source: 'https://example.com/video.mp4' }),
    new Creatomate.Audio({ source: audioUrl }),
  ],
});
```

## Complete AI Video Pipeline

```javascript
async function createFullAIVideo(topic) {
  // 1. Generate script
  const script = await chatGPT(`Write a 30-second video script about ${topic}`);

  // 2. Generate voiceover
  const audioUrl = await generateSpeech(script.narration);

  // 3. Generate background images
  const backgrounds = await Promise.all(
    script.scenes.map(scene =>
      generateBackgroundWithDALLE(scene.visualDescription)
    )
  );

  // 4. Build video
  const elements = script.scenes.map((scene, i) => ({
    background: backgrounds[i],
    text: scene.text,
    duration: scene.duration,
  }));

  // 5. Render
  const source = buildVideoSource(elements, audioUrl);
  const client = new Creatomate.Client(apiKey);

  return await client.render({ source });
}
```

## See Also

- Complete ChatGPT example: `repo/chatgpt/index.js`
- AWS Polly example: `repo/aws-polly/index.js`
- Text-to-speech explainer: `repo/text-to-speech/index.js`
