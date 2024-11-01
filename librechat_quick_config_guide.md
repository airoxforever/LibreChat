# LibreChat Quick Configuration Guide

## Core Features Configuration

### 1. Speech Features (STT/TTS)

#### Quick Setup for Speech-to-Text
```yaml:librechat.yaml
speech:
  speechTab:
    conversationMode: true  # Enable conversation mode
    speechToText:
      engineSTT: "browser"  # Simplest option, uses browser's built-in STT
      autoTranscribeAudio: true
      autoSendText: 0  # Set to desired delay in ms, 0 for manual send
```

#### Advanced STT Options
```yaml:librechat.yaml
speech:
  stt:
    openai:  # OpenAI Whisper
      apiKey: '${STT_API_KEY}'
      model: 'whisper-1'
```

#### Text-to-Speech Setup
```yaml:librechat.yaml
speech:
  speechTab:
    textToSpeech:
      engineTTS: "browser"  # Simplest option
      automaticPlayback: true
      playbackRate: 1.0
```

### 2. Model Access Configuration

#### OpenRouter Setup
```env:.env
OPENROUTER_KEY=your_api_key_here
```

```yaml:librechat.yaml
endpoints:
  custom:
    - name: "OpenRouter"
      apiKey: "${OPENROUTER_KEY}"
      baseURL: "https://openrouter.ai/api/v1"
      models:
        default: ["meta-llama/llama-3-70b-instruct"]
        fetch: true
```

## Student-Focused Configuration

### 1. Interface Simplification
```yaml:librechat.yaml
interface:
  # Hide advanced features
  hideExamplePrompts: true
  hideModelSettings: true
  hidePluginSettings: true
  
  # Customize appearance
  APP_TITLE: "Language Learning Assistant"
  CUSTOM_FOOTER: "Your Language Learning Platform"
```

### 2. Model Restrictions
```yaml:librechat.yaml
endpoints:
  custom:
    - name: "OpenRouter"
      models:
        # Limit to specific models suitable for language learning
        default: ["meta-llama/llama-3-70b-instruct"]
        fetch: false  # Prevent showing all available models
```

### 3. Safety Settings
```yaml:librechat.yaml
# Enable content moderation
moderation:
  enabled: true
  filterLevel: "medium"  # Options: low, medium, high

# Rate limiting
rateLimit:
  messages:
    windowMs: 900000  # 15 minutes
    max: 50  # Maximum messages per window
```

## Quick Tips

1. **Environment Variables**: Most sensitive settings (API keys, credentials) should be in `.env`
2. **Configuration Updates**: 
   - For Hugging Face Spaces: Update through the control panel
   - For self-hosted: Modify `librechat.yaml` and restart the service
3. **Testing Changes**: Always test configuration changes in a development environment first

## Common Issues

1. **Speech Recognition Not Working**
   - Check if browser permissions are granted
   - Verify API keys if using cloud services
   - Ensure proper SSL/HTTPS setup

2. **Model Access Issues**
   - Verify API keys
   - Check model availability in your region
   - Confirm quota/credit availability

## Recommended Settings for Language Learning

1. **Speech Recognition Priority**
```yaml:librechat.yaml
speech:
  speechTab:
    conversationMode: true
    speechToText:
      engineSTT: "browser"
      autoTranscribeAudio: true
      decibelValue: -45  # Adjust sensitivity
```

2. **Response Configuration**
```yaml:librechat.yaml
conversation:
  # Optimize for language learning
  temperature: 0.7  # Balance between creativity and accuracy
  maxTokens: 150    # Keep responses concise
  systemPrompt: "You are a helpful language learning assistant..."
```

3. **Deployment and Updates Process**

There are three main ways to update your HuggingFace Space:

A. **Through Variables/Secrets Panel**:
- Changes take effect after clicking "Restart Space"
- Best for configuration changes
- No code modifications needed

B. **Through GitHub Integration**:
1. Connect your GitHub repo to HuggingFace Space
2. Edit files in your GitHub repo
3. Push changes to the main/master branch
4. HuggingFace will automatically rebuild the Space

C. **Direct File Edit in Space**:
1. Go to "Files" tab in your Space
2. Edit files directly
3. Click "Restart Space" to apply changes

For your use case, I recommend:
1. Use Variables/Secrets panel for API keys and basic configuration
2. Use GitHub for any code/file modifications
3. Create a `librechat.yaml` in your repo for advanced settings

Would you like me to provide more details about any of these aspects? I can also help you set up the GitHub integration if needed.

Also, regarding OpenRouter - yes, you can add it as a secret! It's actually recommended to add API keys as secrets rather than variables for better security.

## HuggingFace Space Configuration

### Required Variables and Secrets

#### Secrets (Sensitive Information)
```plaintext
OPENAI_API_KEY         # Required for OpenAI models and Whisper STT
OPENROUTER_KEY         # Required for OpenRouter access
GOOGLE_KEY             # Required for Gemini models
```

#### Variables (Configuration Settings)
```plaintext
ENDPOINTS              # Set to: openAI,google,custom
PORT                   # Set to: 3000
HOST                   # Set to: 0.0.0.0
ALLOW_EMAIL_LOGIN      # Set to: true
ALLOW_REGISTRATION     # Set to: true (if you want users to register)
DEBUG_LOGGING          # Set to: true (helps with troubleshooting)
```

### GitHub Integration Setup

To connect/reconnect your Space to GitHub:

1. Go to your Space settings by clicking the ⚙️ icon
2. Look for "Repository" section
3. Click "Visit repository" to see if there's an existing connection
4. If no connection exists:
   - Click "Configure repository"
   - Choose "Import from Git"
   - Enter your GitHub repository URL
   - Select the branch you want to deploy (usually 'main' or 'master')

To verify the connection:
1. Go to the "Files" tab in your Space
2. Look for a small GitHub icon next to the files
3. Check the "Logs" tab for deployment activities

#### Automatic Rebuilds
To ensure your Space rebuilds when you push to GitHub:
1. Go to your Space settings
2. Look for "Build Triggers" section
3. Enable "Rebuild on source code update"

#### Manual Rebuild
If automatic rebuilds aren't working:
1. Go to your Space settings
2. Find the "Factory reset" section
3. Click "Factory reset" to rebuild from your GitHub source

> Note: Factory reset will rebuild the entire Space from scratch using your GitHub repository as the source. This is useful when you want to ensure all your GitHub changes are applied.

Would you like me to add more details about any of these sections?