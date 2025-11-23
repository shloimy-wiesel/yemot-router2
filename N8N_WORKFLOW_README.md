# n8n Email to Yemot Workflow

This n8n workflow automatically uploads audio files from Gmail emails to a Yemot phone system line.

## Workflow Overview

The workflow performs the following steps:

1. **Gmail Trigger**: Monitors emails from `placeholder@gmail.com` that contain "Yemot" in the subject
2. **Check Attachments**: Verifies that the email has attachments
3. **Split Attachments**: Splits multiple attachments into individual items for processing
4. **Check Audio File**: Filters only audio files (all audio/* MIME types)
5. **Upload to Yemot Line 8**: Uploads each audio file to Yemot line number 8

## Setup Instructions

### 1. Import the Workflow

1. Open your n8n instance
2. Click on "Workflows" → "Import from File"
3. Select `n8n-email-to-yemot-workflow.json`

### 2. Configure Gmail Credentials

1. Click on the "Gmail Trigger" node
2. Under "Credentials", click "Create New"
3. Follow n8n's OAuth2 setup for Gmail
4. Grant the necessary permissions

### 3. Configure Yemot Credentials

1. Click on the "Upload to Yemot Line 8" node
2. Replace the placeholder values in the body parameters:
   - `username`: Your Yemot username
   - `password`: Your Yemot password
   - `path`: Currently set to `ivr2:/$Path/8` (line 8). Modify if needed.

### 4. Update Email Filter

1. Click on the "Gmail Trigger" node
2. Update the `sender` field from `placeholder@gmail.com` to the actual email address
3. Optionally modify the `includeSubject` filter

### 5. Activate the Workflow

1. Click the toggle switch at the top to activate the workflow
2. The workflow will check for new emails every minute

## Workflow Features

### ✅ Handles Multiple Audio Files
If an email contains multiple audio attachments, each one will be uploaded separately to Yemot line 8.

### ✅ Audio Format Support
The workflow supports all audio formats with MIME types starting with `audio/`, including:
- MP3 (`audio/mpeg`)
- WAV (`audio/wav`)
- OGG (`audio/ogg`)
- M4A (`audio/mp4`)
- And more

### ✅ Smart Filtering
- Only processes emails from the specified sender
- Only processes emails with "Yemot" in the subject
- Skips emails without attachments
- Skips non-audio attachments

## Workflow Behavior

### When Email Has No Attachments
The workflow will output "No audio attachments found" and stop processing.

### When Email Has Non-Audio Attachments
The workflow will skip non-audio files and output "Not an audio file" for each skipped attachment.

### When Email Has Audio Attachments
Each audio file will be uploaded to Yemot line 8, and you'll see the API response for each upload.

## Troubleshooting

### Workflow Not Triggering
- Verify Gmail credentials are properly configured
- Check that the email sender matches exactly
- Ensure the workflow is activated (toggle switch is on)

### Upload Failures
- Verify Yemot username and password are correct
- Check that the path `ivr2:/$Path/8` is correct for your Yemot system
- Ensure your Yemot account has permission to upload to line 8

### Binary Data Issues
If you get errors about binary data, ensure the "Split Attachments" node is properly configured to handle binary data from Gmail attachments.

## Customization

### Change Target Line Number
To upload to a different line number, edit the "Upload to Yemot Line 8" node:
- Change the `path` parameter from `ivr2:/$Path/8` to your desired line (e.g., `ivr2:/$Path/5` for line 5)

### Add Email Notification
You can add an email notification node after successful uploads to notify yourself when files are uploaded.

### Filter by File Size
Add an additional condition node to filter files by size if needed.

## API Reference

This workflow uses the Yemot UploadFile API:
- **Endpoint**: `https://www.call2all.co.il/ym/api/UploadFile`
- **Method**: POST
- **Content-Type**: multipart/form-data

For more information about Yemot APIs, visit: https://f2.freeivr.co.il/
