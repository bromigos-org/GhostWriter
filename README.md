# GhostWriter

> **Status: inactive since August 2025.** GhostWriter was an early prototype of a
> unified inbox. Only SMS intake (via TextBee) and priority classification were
> built; the other platforms listed below were never implemented, and the last CI
> runs (2025-08-22) failed. Nothing deploys or depends on this code (checked
> 2026-10-05). The bromigos.org homepage still lists it as "In dev" and links
> here. The repo is a candidate for archiving.

GhostWriter is a powerful tool for creating and managing a unified collection of messaging applications. It provides a set of automated action items, based on priority and context, across multiple communication platforms.

## 🚀 Supported Applications

- **Slack** - Team collaboration and messaging
- **Gmail** - Email communication
- **Outlook** - Microsoft email and calendar
- **Discord** - Gaming and community chat
- **Telegram** - Secure messaging
- **SMS** - Text messaging

## 📋 Functional Requirements

- [x] Receive messages from SMS platform (TextBee integration)
- [x] Queue and process messages efficiently
- [x] Identify and set message priorities
- [ ] Receive messages from other supported applications
- [ ] Generate intelligent summaries of message content
- [ ] Generate contextual responses to messages
- [ ] Automate responses based on priority thresholds

## ⚡ Non-Functional Requirements

- [x] Message priority classification system
- [x] Conversation priority management
- [x] Configurable response priority thresholds
- [x] Scalable message processing architecture
- [ ] Secure authentication and data handling

## 🎯 Core Features (v1.0)

### Message Reception & Processing ✅ **SMS IMPLEMENTED**

- [x] **Receive SMS messages via TextBee**
- [x] **Queue messages for processing**
- [x] **Process messages in queue**
- [ ] Receive messages from Slack
- [ ] Receive messages from Discord
- [ ] Receive messages from Gmail

### Intelligence & Automation ✅ **PRIORITY SYSTEM IMPLEMENTED**

- [x] **Identify priority level of messages**
- [x] **Advanced priority classification with urgency scoring**
- [x] **Context tag extraction (financial, meeting, security, etc.)**
- [x] **Time-based urgency adjustments**
- [ ] Generate contextual responses based on service and message context
- [ ] Reply to messages below configurable priority levels
- [ ] Generate comprehensive message summaries
- [ ] Create calendar events from messages when appropriate

### Message Distribution

- [x] **Send SMS messages via TextBee**
- [ ] Send messages to Slack
- [ ] Send messages to Discord
- [ ] Send messages to Gmail

## 🔮 Future Features

- [ ] Create tasks from messages when appropriate
- [ ] Create notes from messages when appropriate
- [ ] Create contacts from messages when appropriate
- [ ] Create emails from messages when appropriate
- [ ] Advanced natural language processing for better context understanding
- [ ] Integration with project management tools
- [ ] Custom workflow automation rules
- [ ] Analytics and reporting dashboard

## 🛠️ Getting Started

### Prerequisites

- Python 3.12 or higher
- Poetry (for dependency management)
- TextBee account and Android device for SMS integration

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/bromigos-org/GhostWriter.git
   cd GhostWriter
   ```

2. Install dependencies:
   ```bash
   poetry install
   ```

3. Set up configuration:
   ```bash
   cp .env.example .env
   # Edit .env with your TextBee API credentials
   ```

4. Activate the virtual environment:
   ```bash
   poetry shell
   ```

### TextBee SMS Setup

1. **Create TextBee Account**: Sign up at [textbee.dev](https://textbee.dev)
2. **Install Android App**: Download from [dl.textbee.dev](https://dl.textbee.dev)
3. **Connect Device**: Use QR code or manual API key entry
4. **Configure GhostWriter**: Add your API key and device ID to `.env`

### Running GhostWriter

```bash
# Run the application
poetry run ghostwriter

# Or run directly
poetry run python -m ghostwriter.main
```

### Current Implementation Status

🎉 **WORKING FEATURES:**
- ✅ **SMS Integration**: Full TextBee SMS platform integration
- ✅ **Message Processing**: Asynchronous message queue processing with proper shutdown handling
- ✅ **Priority Classification**: AI-powered priority scoring with urgency levels
- ✅ **Context Analysis**: Automatic tag extraction (financial, security, meeting, etc.)
- ✅ **Real-time Monitoring**: Configurable polling intervals
- ✅ **Error Handling**: Robust error handling and retry logic
- ✅ **Development Tools**: Ruff linting, MyPy type checking, and pre-commit hooks
- ✅ **Signal Handling**: Graceful shutdown on SIGINT/SIGTERM

📱 **SMS Features:**

- Receive SMS messages in real-time
- Send SMS replies through your Android device
- Priority scoring based on content, time, and context
- Deduplication to prevent processing the same message twice
- Context tags for financial, security, meeting-related messages

### Development

```bash
# Install development dependencies
poetry install --with dev

# Code quality checks
poetry run ruff check src tests          # Linting
poetry run ruff format src tests         # Formatting
poetry run mypy src                      # Type checking

# Run all quality checks
poetry run ruff check src tests && poetry run ruff format --check src tests && poetry run mypy src

# Pre-commit hooks (one-time setup)
poetry run pre-commit install

# Run pre-commit manually
poetry run pre-commit run --all-files

# Run tests (when implemented)
poetry run pytest

# Add new dependencies
poetry add package_name

# Add development dependencies
poetry add --group dev package_name
```

## 📄 License

This project is licensed under the terms specified in the [LICENSE](LICENSE) file.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

---

**GhostWriter** - Unifying your messaging experience across platforms.
