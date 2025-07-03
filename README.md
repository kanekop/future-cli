# the-future.sh

> A tiny CLI simulator to boot your future.

`the-future.sh` is not just a tool; it's an interactive web experience designed to inspire action. It frames the abstract idea of building a future in the familiar language of a command-line interface, reminding us that a vision, without a commit, is just a dream.

_(You can replace this with a link to your own screenshot)_

## Philosophy

The future isn't something you wait for; it's something you build, one commit at a time. This project embodies that philosophy through deliberate design choices:

- **Friction by Design**: Simply running `./boot_future.sh` results in an error. This isn't a bug—it's the core message. Your future requires explicit vision and commitment.
- **Learning Through Failure**: The error-to-success journey mirrors real life. Understanding what's needed is part of the process.
- **Authentic Achievement**: When you finally boot your future with the right parameters, the success feels earned.

## Features

This project is a single, self-contained `index.html` file, but it's packed with features to create a compelling and shareable experience.

- **Realistic Terminal Interface**: A CLI-like environment simulated entirely in your browser, featuring a blinking cursor, colored output, and a familiar prompt (`$`).
    
- **The Boot Command**: The core of the experience. Users must provide two critical flags to boot their future:
    
    - `--vision`: A string describing the future you want to build.
        
    - `--commit`: A string describing the first, concrete action you will take.
        
- **Interactive Mode (Training Mode)**: For those new to CLI, there's a guided mode that teaches the proper command structure:
    - Accessed via `./boot_future.sh --interactive`
    - Walks users through the process step-by-step
    - Shows the correct command format at the end
    - Generated images include a "TRAINING MODE" watermark
    - Designed as a learning tool, not a replacement for the real experience
        
- **Success & Failure States**:
    
    - If the command is run without the required flags, a `[FATAL]` error is displayed, along with a `USAGE` guide and a philosophical tip.
        
    - On successful execution, the app displays a log of your future being initiated, including the iconic `Daemonizing... Your future is now running in the background. Keep committing.` message.
        
- **Command History**: Just like a real terminal, you can navigate through your past commands using the `ArrowUp` and `ArrowDown` keys. The initial `./boot_future.sh` command is pre-loaded into the history for convenience.
    
- **Perfectly Cropped Snapshot**: After a successful boot, a shareable image (`.png`) is automatically generated. This snapshot is intelligently cropped to include the entire story of your session—from the initial "error" message to your successful command execution—with no wasted space.
    
- **Easy Sharing**: A share box appears at the top of the screen with two options:
    
    1. **Download Image**: Save the generated snapshot directly to your device.
        
    2. **Share on X**: Opens the X (Twitter) intent window with a pre-populated template, ready for you to share your commitment. The button order is intentionally designed to encourage downloading the image first.
        

## How to Use

### Standard Mode (Recommended)

1. Visit the website.
    
2. You will be greeted by an initial error message, prompting you for action.
    
3. At the prompt, type your command using the following format. You can use single (`'`) or double (`"`) quotes.
    
    ```
    ./boot_future.sh --vision "a world where everyone can create" --commit "start learning to code today"
    ```
    
    Note: Both `./boot_future.sh` and `boot_future.sh` are accepted, though the former follows proper shell conventions for executing local scripts.
    
4. Press `Enter`.
    
5. Watch your future boot.
    
6. Use the buttons at the top to download your personal snapshot and share it with the world.
    

### Interactive Mode (Training)

For those unfamiliar with command-line interfaces:

1. Type `./boot_future.sh --interactive`
    
2. Follow the guided prompts to enter your vision and commitment
    
3. The system will show you the proper command format
    
4. Confirm to execute and see your future boot
    
Note: Interactive mode is designed as a learning tool. Images generated in this mode will include a "TRAINING MODE" indicator. For the authentic experience and shareable images without watermarks, use the standard command format.

### Shell Environment Commands

To enhance the CLI experience, the following standard shell commands are also available:

```bash
ls          # List files in current directory
cat         # Display file contents
clear       # Clear the terminal
pwd         # Print working directory
whoami      # Display current user
help        # Show help message
```

These commands make the environment feel more authentic and help users familiarize themselves with basic shell operations.

## Command Examples

```bash
# Standard execution (recommended)
./boot_future.sh --vision "sustainable cities everywhere" --commit "bike to work this week"

# Alternative format (also works)
boot_future.sh --vision "sustainable cities everywhere" --commit "bike to work this week"

# Get help
./boot_future.sh --help

# Interactive mode (for learning)
./boot_future.sh --interactive

# Basic shell commands
ls          # See available files
help        # Get command help
clear       # Clean up the terminal
```

## The Design Philosophy

This tool intentionally requires effort. In a world of one-click solutions, `the-future.sh` asks you to slow down and think. The command-line interface isn't just an aesthetic choice—it's a reminder that building a future requires:

1. **Clear Intent**: You must explicitly state your vision
2. **Concrete Action**: You must commit to a specific first step
3. **Personal Effort**: You must type it out yourself

The "error-first" design ensures that every successful boot feels meaningful. This isn't about making things difficult—it's about making them memorable.

## Tech Stack

- **Vanilla HTML, CSS, JavaScript**: No frameworks. Just the fundamentals.
    
- [**Typed.js**](https://github.com/mattboldt/typed.js/ "null"): For the typing animation effect.
    
- [**html2canvas**](https://github.com/niklasvh/html2canvas "null"): For generating the image snapshot from DOM elements.
    
- **Deployment**: Hosted on [Vercel](https://vercel.com/ "null").

## Future Development

While the core experience is intentionally minimal, we're exploring ways to enhance accessibility without compromising the core message. The following features are under consideration:

### 🌐 Internationalization (i18n)

Automatic language detection based on the content of vision/commit inputs:
- Japanese interface when Japanese characters are detected
- English interface as default
- Seamless switching without manual configuration

### 🤖 AI-Powered Vision Generator (Under Consideration)

For those who have a concrete action but struggle to see the bigger picture:

```bash
$ ./boot_future.sh --ai-vision --commit "practice guitar for 30 minutes daily"

[AI VISION GENERATOR] Analyzing your commit...

Your daily 30-minute guitar practice could lead to:
- 1 month: Basic chord mastery and rhythm improvement
- 1 year: Performance-ready skills and musical expression
- 3 years: Inspiring others through music, possibly teaching

Suggested vision: "A world where everyone expresses their soul through music"

Use this vision? (y/n):
```

**Technical Approach:**
- Minimal API calls with efficient prompts (target: <100 tokens per request)
- Local caching of common patterns to reduce costs
- Privacy-first: commits are processed but never stored

### 🎯 Extended Options (Under Consideration)

Additional optional flags to deepen commitment without overwhelming simplicity:

```bash
# Core (Required) - Unchanged
--vision "your future vision"
--commit "your immediate action"

# Extended (Optional)
--when "2024-12-31"              # Deadline for starting
--first-step "download Python"    # The very first micro-action  
--why "for my children"          # Personal motivation
--milestone "app launch in 3mo"  # Intermediate goal
```

**Impact on Output:**
The more details provided, the richer the boot sequence:

```
# Minimal (vision + commit only)
Future process started successfully. (PID: 20241225)
Daemonizing... Your future is now running in the background.

# Detailed (with optional flags)
Future process started successfully. (PID: 20241225)
├── First checkpoint: Download Python by tonight
├── Milestone: App launch in 3 months
├── Driven by: For my children
└── Deadline: 2024-12-31
Daemonizing... Your future is now running in the background.
```

### 📋 Additional Planned Features

- **Command history persistence**: Remember your past futures across sessions
- **Achievement system**: Unlock easter eggs for power users
- **Export formats**: Generate wallpapers, calendar events, or reminder cards
- **Community futures**: Anonymously share and discover inspiring vision/commit pairs

### Implementation Timeline

1. **Phase 1 (Current)**: Interactive mode implementation
2. **Phase 2**: Internationalization (Japanese/English)
3. **Phase 3**: Extended options system
4. **Phase 4**: AI integration (pending cost analysis)

> **Note**: AI-powered features and extended options are still under consideration. We're carefully evaluating how to implement these without diluting the core message that "futures require deliberate effort." The timeline and final feature set may change based on community feedback and testing.

What's your first commit?