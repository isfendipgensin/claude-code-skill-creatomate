---
name: templates
description: Creatomate template rendering with dynamic modifications and batch generation
---

# Templates

Templates are pre-designed compositions created in the Creatomate Template Editor. Use templates when you need complex designs with consistent branding.

## Creating Templates

1. Go to [Creatomate.com](https://creatomate.com)
2. Open the Template Editor
3. Design your video/image
4. Note the Template ID from the URL or project settings

## Rendering a Template

```javascript
const Creatomate = require('creatomate');
const client = new Creatomate.Client('YOUR_API_KEY');

const renders = await client.render({
  // Template ID from your Creatomate account
  templateId: '2e8bccbf-e40a-41d5-a815-f58518ed9835',

  // Values to insert into template placeholders
  modifications: {
    'Title': 'Your headline here',
    'Text 1': 'First paragraph of content',
    'Text 2': 'Second paragraph of content',
  },
});

console.log('Video URL:', renders[0].url);
```

## Modifications Object

The `modifications` object maps template element names to new values:

### Text Modifications

```javascript
modifications: {
  'Title': 'New title text',
  'Subtitle': 'New subtitle',
}
```

### Media Source Modifications

Replace images/videos by targeting the `.source` property:

```javascript
modifications: {
  'Image-1.source': 'https://example.com/new-image.jpg',
  'Video-1.source': 'https://example.com/new-video.mp4',
}
```

### Color Modifications

```javascript
modifications: {
  'Title.fill_color': '#ff0000',
  'Background.fill_color': '#0000ff',
}
```

### Multiple Properties

```javascript
modifications: {
  // Text content
  'Title': 'Main Headline',
  'Body': 'Description text here',

  // Media
  'Hero-Image.source': 'https://example.com/hero.jpg',
  'Logo.source': 'https://example.com/logo.png',

  // Colors
  'Accent.fill_color': '#ff6b00',
}
```

## Source Editor (JSON Import)

To use templates from the node-examples:

1. Create a new blank template in Creatomate
2. Press F12 to open the Source Editor
3. Paste the JSON from `template.json` files
4. Save and get the Template ID

## Template with ChatGPT Example

```javascript
const Creatomate = require('creatomate');
const fetch = require('node-fetch');

async function run() {
  // Get content from ChatGPT
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
        content: '5 fascinating facts about the Golden Eagle. ' +
          'Make a numbered list of 5 short sentences with emojis.'
      }],
    }),
  });

  const data = await response.json();
  const facts = parseFacts(data.choices[0].message.content);

  // Render template with AI-generated content
  const client = new Creatomate.Client(creatomateApiKey);

  const renders = await client.render({
    templateId: 'your-template-id',
    modifications: {
      'Intro-Text': '5 Facts About Golden Eagles',
      'Fact-1': facts[0],
      'Fact-2': facts[1],
      'Fact-3': facts[2],
      'Fact-4': facts[3],
      'Fact-5': facts[4],
    },
  });

  console.log('Video URL:', renders[0].url);
}
```

## Multiple Renders from One Template

Render multiple versions with different data:

```javascript
const renders = await client.render([
  {
    templateId: 'template-id',
    modifications: { 'Title': 'Version 1' },
  },
  {
    templateId: 'template-id',
    modifications: { 'Title': 'Version 2' },
  },
  {
    templateId: 'template-id',
    modifications: { 'Title': 'Version 3' },
  },
]);

// renders is an array with all 3 results
renders.forEach((render, i) => {
  console.log(`Version ${i + 1}:`, render.url);
});
```

## Tags for Multiple Outputs

Use tags to generate multiple aspect ratios from one template:

```javascript
const renders = await client.render({
  templateId: 'template-id',
  modifications: { 'Title': 'My Video' },
  tags: ['landscape', 'portrait', 'square'],
});

// Returns renders for each tagged output configuration
```

## Webhook Notification

Get notified when rendering completes:

```javascript
const renders = await client.render({
  templateId: 'template-id',
  modifications: { 'Title': 'My Video' },
  webhookUrl: 'https://yourserver.com/creatomate-webhook',
  metadata: 'order-123',  // Custom tracking data
});
```
