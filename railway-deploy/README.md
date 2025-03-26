# AI News Scraper Auto Blogger Pro - Railway.app Deployment

This folder contains the configuration files for deploying the AI News Scraper Auto Blogger Pro API on Railway.app.

## Overview

AI News Scraper Auto Blogger Pro is a WordPress plugin that automatically scrapes news articles, rewrites them using AI, and publishes them to WordPress blogs with SEO optimization. This API service provides the backend functionality for the plugin.

## Features

- **Web Scraping**: Extract clean content from any web page
- **RSS Feed Processing**: Monitor and process articles from RSS feeds
- **AI Content Generation**: Create unique content using various AI models:
  - OpenAI GPT-4o
  - Google Gemini
  - Anthropic Claude
  - DeepSeek
- **Image Generation**: Create featured images using DALL-E
- **SEO Optimization**: Generate SEO metadata, tags, and categories

## Deployment on Railway.app

### Prerequisites

1. A Railway.app account
2. The Railway CLI installed (optional, for command-line deployment)

### Deployment Steps

#### Method 1: Using Railway Dashboard

1. Fork or clone this repository
2. Log in to your Railway.app dashboard
3. Click "New Project" → "Deploy from GitHub repo"
4. Select this repository and the branch you want to deploy
5. Railway will automatically detect the configuration and deploy your app

#### Method 2: Using Railway CLI

1. Install the Railway CLI:
   ```bash
   npm i -g @railway/cli
   ```

2. Log in to Railway:
   ```bash
   railway login
   ```

3. Initialize a new project:
   ```bash
   railway init
   ```

4. Deploy the app:
   ```bash
   railway up
   ```

### Environment Variables

Set the following environment variables in your Railway.app project:

- `PORT`: The port on which the application will run (automatically set by Railway)
- `OPENAI_API_KEY`: Your OpenAI API key (required for OpenAI and DALL-E functionality)
- `GEMINI_API_KEY`: Your Google Gemini API key (optional)
- `CLAUDE_API_KEY`: Your Anthropic Claude API key (optional)
- `DEEPSEEK_API_KEY`: Your DeepSeek API key (optional)

## Local Development

1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

2. Run the application:
   ```bash
   python main.py
   ```

The application will be available at `http://localhost:5000`.

## API Documentation

### Endpoints

- `GET /status`: Health check endpoint
- `POST /scrape`: Scrape content from a URL
- `POST /scrape/rss`: Scrape articles from an RSS feed
- `POST /scrape/rss/test`: Test an RSS feed by fetching a few items
- `POST /ai/generate`: Generate AI content from scraped content
- `POST /ai/generate_image`: Generate an image using AI
- `POST /ai/generate_seo`: Generate SEO data for an article

### Example Request

```json
POST /ai/generate
{
  "content": "Original content to rewrite...",
  "title": "Original Article Title",
  "model": "openai",
  "api_key": "your-api-key",
  "language": "english",
  "tone": "default",
  "min_word_count": 500,
  "keyword_density": 2.5,
  "auto_headings": true,
  "use_lists": true,
  "add_faq": true,
  "add_conclusion": true
}
```

## License

This project is licensed under the MIT License.

## Support

For support, please visit our website or contact us at support@example.com.