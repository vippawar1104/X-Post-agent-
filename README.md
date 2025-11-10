# Xpostagent

An automated n8n workflow that generates AI-powered images and posts them to X (Twitter). The system uses Google Gemini to generate creative ideas, BytePlus API to create images, and automatically shares them on social media.

## Demo

### Workflow Screenshot

![Xpostagent Workflow](XPostagent.png)

### Video Demonstration

[Watch the workflow in action](https://www.youtube.com/watch?v=YOUR_VIDEO_ID)

## Overview

Xpostagent is an end-to-end automation workflow built with n8n that:

1. Reads existing content from a Google Sheet
2. Uses AI to generate new creative ideas
3. Creates photorealistic images using BytePlus image generation API
4. Stores the generated content back to Google Sheets
5. Automatically posts the images to X (Twitter)

## Features

- Automated content idea generation using Google Gemini
- AI-powered image generation with BytePlus Seedream model
- Google Sheets integration for tracking generated content
- Automatic social media posting to X (Twitter)
- Structured output parsing for consistent data handling
- Error handling and data validation

## Prerequisites

Before setting up this workflow, you need:

- n8n instance (self-hosted or cloud)
- Google Cloud account with Sheets API enabled
- Google Gemini API access
- BytePlus API key
- X (Twitter) Developer account with OAuth credentials

## Required Credentials

Set up the following credentials in n8n:

1. **Google Sheets OAuth2 API**
   - OAuth2 authentication for Google Sheets access
   - Permissions: Read and write to spreadsheets

2. **Google Gemini API**
   - API key for Gemini language model access

3. **BytePlus API**
   - API key for image generation service

4. **X (Twitter) OAuth1 API**
   - OAuth1 authentication for media uploads

5. **X (Twitter) OAuth2 API**
   - OAuth2 authentication for posting tweets

## Installation

### Step 1: Import the Workflow

1. Open your n8n instance
2. Navigate to Workflows
3. Click "Add Workflow" > "Import from File"
4. Select the provided JSON file
5. The workflow will be imported with all nodes configured

### Step 2: Configure Credentials

Update the following credential references in the workflow:

- `YOUR_GOOGLE_SHEETS_CREDENTIAL` - Google Sheets OAuth2 credentials
- `YOUR_GEMINI_API_CREDENTIAL` - Google Gemini API credentials
- `YOUR_TWITTER_OAUTH_CREDENTIAL` - Twitter OAuth2 credentials
- `YOUR_TWITTER_OAUTH_CREDENTIAL` - Twitter OAuth1 credentials

### Step 3: Update Configuration Parameters

Replace placeholder values:

- `YOUR_SHEET_ID_HERE` - Your Google Sheets document ID
- `YOUR_BYTEPLUS_API_KEY` - Your BytePlus API key

## Workflow Structure

### Node Breakdown

1. **When clicking 'Execute workflow'**
   - Manual trigger to start the workflow

2. **Get row(s) in sheet**
   - Reads existing data from Google Sheets
   - Retrieves all previously generated objects

3. **Aggregate**
   - Combines all rows into a single data structure

4. **Set Object List**
   - Prepares the data for AI processing

5. **Idea Agent**
   - Uses Google Gemini to generate new creative ideas
   - Ensures uniqueness by comparing against existing objects
   - Outputs structured JSON with object name and caption

6. **Structured Output Parser**
   - Validates and parses the AI output into structured format

7. **Google Gemini Chat Model**
   - Language model configuration for idea generation

8. **AI Agent**
   - Generates detailed image prompts for the selected object
   - Creates ASMR-style glass cutting scene descriptions

9. **Generate Image**
   - Calls BytePlus API to create the image
   - Uses Seedream-4 model for photorealistic output
   - Returns image in 2K resolution

10. **Extract Image URL**
    - JavaScript code to parse API response
    - Extracts the generated image URL

11. **Append Row in Sheet**
    - Saves the new object and image URL to Google Sheets
    - Maintains a log of all generated content

12. **HTTP Request (Fetch Image)**
    - Downloads the generated image as binary data

13. **Upload Media to X (Twitter)**
    - Uploads the image to Twitter's media service
    - Returns media ID for tweet attachment

14. **Create Tweet**
    - Posts the tweet with the attached image
    - Uses predefined caption text

## Configuration

### Google Sheets Setup

Create a Google Sheet with the following structure:

| Column A | Column B |
|----------|----------|
| objects  | URL      |

The workflow will:
- Read all existing objects from column A
- Append new entries with both object name and image URL

### Customizing AI Prompts

#### Idea Generation (Idea Agent node)

Modify the system message to change the content generation theme:

```
You are an AI agent that selects unique fruits for ASMR-style glass cutting images. Generate one new fruit not on the list.
```

#### Image Prompt Generation (AI Agent node)

Adjust the system message for different image styles:

```
You are an AI that generates photorealistic image prompts for ASMR-style glass cutting scenes.
```

### Image Generation Parameters

In the "Generate Image" node, you can modify:

- `model`: Image generation model (default: seedream-4-0-250828)
- `size`: Output resolution (options: 1K, 2K, 4K)
- `response_format`: Output format (url or base64)

### Tweet Customization

Edit the "Create Tweet" node to change the tweet text:

```
AI just dropped a new masterpiece
```

## Usage

### Manual Execution

1. Open the workflow in n8n
2. Click "Execute Workflow" button
3. Monitor the execution progress
4. Check your X (Twitter) account for the posted content

### Scheduled Execution

To automate the workflow:

1. Replace the "Manual Trigger" node with a "Schedule Trigger" node
2. Configure the desired frequency (hourly, daily, etc.)
3. Activate the workflow

## Error Handling

The workflow includes error handling for:

- Invalid JSON responses from APIs
- Missing image URLs
- Failed API calls
- Network timeouts

Check the execution logs in n8n for detailed error messages.

## Troubleshooting

### Common Issues

**Issue**: Google Sheets node fails
- Verify OAuth2 credentials are valid
- Check that the Sheet ID is correct
- Ensure the sheet name matches (default: "Sheet1")

**Issue**: Image generation fails
- Verify BytePlus API key is valid and has credits
- Check that the prompt doesn't violate content policies
- Review API rate limits

**Issue**: Twitter posting fails
- Confirm both OAuth1 and OAuth2 credentials are configured
- Verify API access level (need elevated access for media uploads)
- Check image file size limits (max 5MB for Twitter)

**Issue**: AI agent produces invalid output
- Review the system prompts for clarity
- Check the structured output parser schema
- Verify Gemini API quota

## API Rate Limits

Be aware of the following rate limits:

- **Google Sheets API**: 60 requests per minute per user
- **Google Gemini**: Varies by tier (check your quota)
- **BytePlus**: Depends on your subscription plan
- **X (Twitter) API**: 50 tweets per 24 hours (standard access)

## Data Privacy

This workflow:
- Stores generated content in your Google Sheets
- Posts content publicly to X (Twitter)
- Sends prompts to third-party AI services (Gemini, BytePlus)

Ensure you comply with applicable data protection regulations.

## Contributing

To improve this workflow:

1. Fork the workflow in n8n
2. Make your modifications
3. Test thoroughly
4. Export and share the updated JSON

## License

This project is provided as-is for educational and commercial use.

## Support

For issues and questions:
- Check n8n community forums
- Review API documentation for each service
- Verify credential configurations

## Acknowledgments

Built with:
- n8n workflow automation
- Google Gemini AI
- BytePlus image generation
- X (Twitter) API

## Version History

- **v1.0.0** - Initial release with full automation pipeline
