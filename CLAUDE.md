# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

the-future.sh is a single-page web application that simulates a terminal interface to inspire users to articulate their vision for the future and commit to concrete action. The entire application is contained in `index.html`.

## Project Structure

```
/
├── index.html    # Main application (HTML, CSS, and JavaScript)
└── README.md     # Project documentation
```

## Development Commands

This is a static HTML project with no build process:
- **Run locally**: Open `index.html` in a web browser or serve with any static file server (e.g., `python -m http.server`)
- **Deploy**: Can be deployed to any static hosting service (project mentions Vercel)

## Key Technical Details

### External Dependencies
The project uses CDN-hosted libraries:
- **html2canvas v1.4.1**: For generating shareable PNG snapshots
- **Typed.js v2.1.0**: Included but not actively used in current implementation
- **Google Fonts**: Roboto Mono font

### Core Functionality

The application simulates a terminal that accepts a single command:
```bash
boot --vision "<your vision>" --commit "<your first step>"
```

Key JavaScript components in `index.html`:
- **Terminal simulation**: Lines 144-368
- **Command parsing**: Validates required `--vision` and `--commit` flags
- **Error handling**: Shows usage instructions for incorrect commands
- **Success animation**: Boot sequence with fake PID generation
- **Snapshot generation**: Creates shareable images using html2canvas
- **Command history**: Arrow key navigation through previous commands

### CSS Architecture

Uses CSS custom properties for theming:
- Terminal-style colors and typography
- Responsive design with mobile considerations
- Animation sequences for errors and boot process

## Important Implementation Notes

1. The terminal accepts only one specific command format with both flags required
2. Error messages use a red flash animation for visual feedback
3. The boot sequence includes multiple animated steps with delays
4. Generated snapshots include the user's vision and commit text
5. Social sharing targets X (Twitter) with pre-filled text