# LiveKit Voice Agent - AI Avatar IT Support Assistant

A voice-enabled AI support assistant built with LiveKit that provides real-time IT support through an interactive avatar. The agent can help users troubleshoot software issues, unblock accounts, send email notifications, and provide support ticket documentation.

## Features

- 🎤 **Voice Interaction**: Real-time voice conversations with speech-to-text and text-to-speech capabilities
- 👤 **AI Avatar**: Interactive avatar powered by Beyond Presence (bey) for visual engagement
- 🛠️ **IT Support Tools**:
  - Unblock user accounts
  - Send email notifications and support ticket summaries
  - Screen sharing support for visual troubleshooting
- 🧠 **Intelligent Assistance**: Powered by OpenAI GPT-4.1-mini for natural conversations
- 🔊 **Audio Enhancements**:
  - Noise cancellation for clear audio
  - Background audio player with thinking sounds
  - Voice activity detection (VAD)
- 📧 **Email Integration**: Automated email sending for support ticket documentation
- 🔌 **External API Integration**: Connect to external services for HR policy queries

## Prerequisites

- Python 3.13 or higher
- [uv](https://github.com/astral-sh/uv) package manager
- LiveKit account and credentials
- Beyond Presence (bey) avatar ID
- OpenAI API key
- Gmail account with App Password (for email functionality)

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd livekit-voice-agent
```

2. Install dependencies using uv:
```bash
uv sync
```

3. Create a `.env.local` file in the project root with the following variables:
```env
# LiveKit Configuration
LIVEKIT_URL=your_livekit_url
LIVEKIT_API_KEY=your_livekit_api_key
LIVEKIT_API_SECRET=your_livekit_api_secret

# Beyond Presence Avatar
BEY_AVATAR_ID=your_avatar_id

# OpenAI Configuration
OPENAI_API_KEY=your_openai_api_key

# Gmail Configuration (for email functionality)
GMAIL_USER=your_email@gmail.com
GMAIL_APP_PASSWORD=your_app_password

# External API (optional)
EXTERNAL_API_URL=http://localhost:5003/api/v1/ask
```

## Project Structure

```
livekit-voice-agent/
├── agent.py              # Main agent implementation with entrypoint
├── main.py               # Alternative agent implementation
├── prompts.py            # Agent instructions and prompts
├── tools.py              # Function tools (unblock_user, send_email)
├── api_call.py           # External API integration
├── pyproject.toml        # Project dependencies and configuration
├── uv.lock               # Dependency lock file
└── .env.local            # Environment variables (create this)
```

## Usage

### Running the Agent

Start the agent server:
```bash
uv run python agent.py
```

Or use the alternative implementation:
```bash
uv run python main.py
```

The agent will:
1. Connect to the LiveKit room
2. Initialize the AI avatar
3. Start listening for voice input
4. Provide IT support assistance

### Agent Capabilities

The assistant can:
- **Troubleshoot Issues**: Guide users through software problems while viewing their shared screen
- **Unblock Users**: Remove user blocks from the system (for GenericCorporateApp)
- **Send Emails**: Automatically send support ticket summaries via email
- **Handle Login Issues**: Assist with username/password format issues
- **Create Tickets**: Document support interactions and send summaries

### Support Workflow

1. User reports an issue
2. Agent asks questions to understand the problem
3. User shares screen (optional) for visual troubleshooting
4. Agent guides through solution steps
5. On success/failure:
   - Agent requests user's email
   - Creates and documents a support ticket
   - Sends email summary with ticket details

## Configuration

### Agent Instructions

The agent behavior is configured in `prompts.py` with detailed instructions for:
- Persona and tone
- Support workflow
- Email templates
- GenericCorporateApp specific support

### Tools

The agent has access to two main tools defined in `tools.py`:

1. **unblock_user(username: str)**: Unblocks a user account by clearing the block file and notifying the client
2. **send_email(to_email: str, subject: str, message: str, cc_email: Optional[str])**: Sends email notifications via Gmail SMTP

### Audio Configuration

- **STT**: AssemblyAI Universal Streaming (English)
- **TTS**: Cartesia Sonic 3
- **VAD**: Silero Voice Activity Detection
- **Noise Cancellation**: BVC (or BVCTelephony for SIP participants)

## Dependencies

- `livekit-agents[bey,hedra,silero,turn-detector]`: Core LiveKit agents framework with plugins
- `livekit-plugins-noise-cancellation`: Noise cancellation plugin
- `httpx`: Async HTTP client for API calls
- `python-dotenv`: Environment variable management
- `requests`: HTTP library

## Development

### Adding New Tools

To add a new function tool, create a function decorated with `@function_tool()` in `tools.py`:

```python
from livekit.agents import function_tool, RunContext

@function_tool()
async def my_new_tool(context: RunContext, param: str) -> str:
    """Tool description for the LLM"""
    # Your implementation
    return "result"
```

Then add it to the `Assistant` class in `agent.py`:
```python
tools=[unblock_user, send_email, my_new_tool]
```

### Modifying Agent Behavior

Edit `prompts.py` to change the agent's instructions, persona, or workflow. The instructions are loaded in `agent.py` when creating the `Assistant` instance.

## Troubleshooting

### Common Issues

1. **Avatar not appearing**: Check that `BEY_AVATAR_ID` is correctly set in `.env.local`
2. **Email sending fails**: Ensure Gmail App Password is used (not regular password) and 2FA is enabled
3. **Audio issues**: Verify noise cancellation settings match your use case (BVC vs BVCTelephony)
4. **API connection errors**: Check that external API URL is correct and service is running

## License

[Add your license here]

## Contributing

[Add contribution guidelines here]

## Support

For issues and questions, please [open an issue](link-to-issues) or contact the development team.

