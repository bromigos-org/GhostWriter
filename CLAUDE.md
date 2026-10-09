# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Plan & Review

### Before starting work

- Always in plan mode to make a plan
- After get the plan, make sure you Write the plan to .claude/tasks/TASK_NAME.md.
- The plan should be a detailed implementation plan and the reasoning behind them, as well as tasks broken down.
- If the task require external knowledge or certain package, also research to get latest knowledge (Use Task tool for research)
- Don't over plan it, always think MVP.
- Once you write the plan, firstly ask me to review it. Do not continue until I approve the plan.

### While implementing

- You should update the plan as you work.
- After you complete tasks in the plan, you should update and append detailed descriptions of the changes you made, so following tasks can be easily hand over to other engineers.

## Project Overview

GhostWriter is a prototype unified inbox. The goal was to triage messages from Slack, Gmail, Outlook, Discord, Telegram and SMS in one place. It has been inactive since August 2025. Only SMS intake (TextBee) and keyword-based priority scoring are built. Discord intake is experimental. README.md has the full feature status.

## Architecture

- **Core Module**: `src/ghostwriter/` contains the main application logic
- **Entry Point**: `src/ghostwriter/main.py:main()` starts `GhostRiderApp` from `core.py`, which polls each enabled platform and passes messages to `processor.py`
- **Platforms**: `src/ghostwriter/platforms/` holds the TextBee SMS client and the experimental Discord OAuth client
- **Storage**: `src/ghostwriter/database/` is SQLite storage used only by the Discord platform
- **Package Structure**: Standard Python package with Poetry for dependency management

## Development Commands

### Environment Setup

```bash
# Install dependencies using Poetry
poetry install

# Activate virtual environment
poetry shell
```

### Running the Application

```bash
# Run directly with Poetry
poetry run ghostwriter

# Or run the module
poetry run python -m ghostwriter.main
```

### Testing

```bash
# The tests CI runs
poetry run pytest tests/test_main.py tests/test_simple.py

# With coverage
poetry run pytest tests/test_main.py tests/test_simple.py --cov=ghostwriter
```

The other test files are stale. `tests/test_message_processor.py` and `tests/test_sms_integration.py` have failing tests, and `tests/test_integration.py` fails to import.

### Development Tools

```bash
# Add new dependencies
poetry add package_name

# Add development dependencies
poetry add --group dev package_name

# Install development dependencies
poetry install --with dev

# Code formatting and linting
poetry run ruff check src tests
poetry run ruff format src tests
poetry run mypy src

# Run all quality checks
poetry run ruff check src tests && poetry run ruff format --check src tests && poetry run mypy src

# Pre-commit setup (one-time)
poetry run pre-commit install

# Run pre-commit manually
poetry run pre-commit run --all-files
```

## SMS Integration with TextBee

### TextBee Setup for Google Pixel

GhostWriter integrates with your Google Pixel phone via TextBee SMS gateway for real-time SMS processing.

#### Step 1: TextBee Account Setup

1. **Create Account**: Register at [textbee.dev](https://textbee.dev)
2. **Download App**: Get the Android app from [dl.textbee.dev](https://dl.textbee.dev)
3. **Install on Pixel**: Install and grant SMS permissions

#### Step 2: Device Connection

**QR Code Method (Recommended):**
1. Go to TextBee Dashboard
2. Click "Register Device"
3. Scan QR with TextBee app on your Pixel

**Manual Method:**
1. Generate API key from dashboard
2. Open TextBee app on Pixel
3. Enter API key manually

#### Step 3: GhostWriter Configuration

Create a `.env` file in the project root:

```bash
# TextBee SMS Configuration
TEXTBEE_API_KEY=your_api_key_here
TEXTBEE_DEVICE_ID=your_device_id_here

# Optional: SMS polling interval (seconds)
SMS__POLLING_INTERVAL=10
```

#### Step 4: Install and Run

```bash
# Install with SMS dependencies
poetry install

# Run GhostWriter
poetry run ghostwriter
```

### SMS Features

- **Real-time SMS Reception**: Polls TextBee API for new messages every 10 seconds
- **SMS Sending**: The SMS client can send through your Pixel phone, but nothing in the app calls it yet
- **Priority Classification**: Automatic urgency scoring for SMS messages
- **Context Analysis**: Extract tags like 'financial', 'meeting', 'security' from SMS content
- **Deduplication**: Skips SMS already seen during the current run (in memory only)

### SMS Priority Rules

- **Urgent**: Keywords like 'urgent', 'emergency', 'help', 'problem'
- **High**: Keywords like 'important', 'deadline', 'meeting', 'payment'
- **Medium**: Default priority for regular messages
- **Low**: Keywords like 'fyi', 'newsletter', 'update'

Additional factors apply only when no keyword matched:
- Short SMS messages (< 50 chars) get higher urgency
- SMS from unknown numbers get lower urgency (there is no contact list yet, so every number is unknown)
- Messages outside business hours (8 AM - 6 PM) get urgency boost

Messages with URLs or phone numbers get context tags, whatever their priority.

### Troubleshooting SMS

**Connection Issues:**
- Verify API key and device ID in `.env` file
- Check TextBee app has SMS permissions on Pixel
- Ensure Pixel has internet connection

**Missing Messages:**
- Check TextBee dashboard for device status
- Verify SMS permissions weren't revoked
- Restart TextBee app if needed

**Rate Limiting:**
- TextBee has rate limits for API calls
- GhostWriter respects polling intervals to avoid limits
- Consider upgrading to TextBee Pro for higher limits

## Key Implementation Areas

Based on the README requirements, the system needs to implement:

1. **Message Reception Layer**: ✅ SMS (TextBee); Discord OAuth is experimental; Slack and Gmail are TODO
2. **Processing Queue**: ✅ Asynchronous message processing system
3. **Intelligence Engine**: ✅ Priority classification and context analysis
4. **Response Generation**: AI-powered contextual responses (TODO)
5. **Action Automation**: Automated responses based on priority thresholds (TODO)

## Current State

Inactive since August 2025 and a candidate for archiving. Nothing deploys or depends on this code.

- SMS intake and priority scoring work end to end.
- CI is red: `ruff format --check` wants to reformat `src/ghostwriter/database/manager.py`.
- The console output still uses the project's earlier name, GhostRider.
