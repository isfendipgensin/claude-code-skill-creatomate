# Creatomate Skill for Claude Code

> Generate videos, images, slideshows, and social media content programmatically with Creatomate API

A [Claude Code](https://claude.ai/claude-code) skill that provides comprehensive guidance for using the [Creatomate](https://creatomate.com) video/image generation API. Build TikTok videos, Instagram Reels, YouTube Shorts, animated slideshows, and more—all from code.

## Features

- **Video Generation** — Concatenate, trim, add overlays, apply effects
- **Social Media** — TikTok, Instagram Stories/Reels, YouTube Shorts formats
- **Slideshows** — Ken Burns effects, transitions, background music
- **Text & Captions** — Animated text, AWS Transcribe captions, subtitles
- **Templates** — Render Creatomate templates with dynamic modifications
- **AI Integration** — ChatGPT content generation, DALL-E backgrounds
- **Effects** — Blur, filters, masks, shadows, color overlays

## Installation

```bash
# Clone into your Claude Code skills directory
git clone https://github.com/Sara-Saraireh/claude-code-skill-creatomate.git \
  ~/.claude/skills/creatomate

# Or for project-specific installation
git clone https://github.com/Sara-Saraireh/claude-code-skill-creatomate.git \
  .agents/skills/creatomate
```

## Usage

Invoke the skill in Claude Code:

```
/creatomate
```

Or Claude Code will auto-detect when you mention:
- Creating videos programmatically
- Creatomate API or SDK
- Video slideshows with transitions
- Social media video generation (TikTok, Reels, Shorts)
- Animated captions or subtitles
- Video templates with dynamic content

## Reference Documentation

| Topic | Description |
|-------|-------------|
| [SDK Basics](references/sdk-basics.md) | Client setup, API key, rendering |
| [Source Elements](references/source-elements.md) | Video, Image, Audio, Text elements |
| [Text Elements](references/text-elements.md) | Fonts, styling, text animations |
| [Video & Audio](references/video-audio.md) | Concatenation, trimming, PiP |
| [Images](references/images.md) | Ken Burns, slideshows, watermarks |
| [Effects](references/effects.md) | Blur, filters, masks, shadows |
| [Animations](references/animations.md) | Keyframes, transitions, easing |
| [Captions](references/captions.md) | AWS Transcribe, animated subtitles |
| [Compositions](references/compositions.md) | Scenes, grouped elements |
| [Templates](references/templates.md) | Template rendering with modifications |
| [Social Media](references/social-media.md) | TikTok, Instagram, YouTube formats |
| [Slideshows](references/slideshows.md) | Image slideshows with transitions |
| [AI Integration](references/ai-integration.md) | ChatGPT, DALL-E integration |

## Code Examples

For 50+ working code examples, see the official Creatomate repository:

**[Creatomate Node.js Examples](https://github.com/Creatomate/node-examples)**

Popular examples:
- [Concatenate videos](https://github.com/Creatomate/node-examples/tree/main/concatenate)
- [Image slideshow](https://github.com/Creatomate/node-examples/tree/main/slideshow)
- [Animated captions](https://github.com/Creatomate/node-examples/tree/main/captions)
- [ChatGPT integration](https://github.com/Creatomate/node-examples/tree/main/chatgpt)
- [Story video (TikTok/Reels)](https://github.com/Creatomate/node-examples/tree/main/story-video)
- [Text overlays](https://github.com/Creatomate/node-examples/tree/main/text-overlay)
- [AWS Polly text-to-speech](https://github.com/Creatomate/node-examples/tree/main/aws-polly)

## Requirements

- [Claude Code CLI](https://claude.ai/claude-code)
- Creatomate API key ([get one free](https://creatomate.com))
- Node.js 16+ (for running examples)

## Keywords

`video-generation` `creatomate` `claude-code` `ai-video` `tiktok` `instagram-reels` `youtube-shorts` `slideshows` `animated-captions` `text-to-video` `chatgpt-integration` `n8n` `video-api` `social-media-automation`

## License

MIT

## Credits

- Skill created for [Claude Code](https://claude.ai/claude-code) by Anthropic
- Based on [Creatomate API](https://creatomate.com/docs) documentation
- Examples from [Creatomate/node-examples](https://github.com/Creatomate/node-examples)
