# the-future.sh

> A tiny CLI simulator to boot your future.

`the-future.sh` is not just a tool; it's an interactive web experience designed to inspire action. It frames the abstract idea of building a future in the familiar language of a command-line interface, reminding us that a vision, without a commit, is just a dream.

_(You can replace this with a link to your own screenshot)_

## Philosophy

The future isn't something you wait for; it's something you build, one commit at a time. This project embodies that philosophy through deliberate design choices:

- **Friction by Design**: Simply running `./future.sh` results in an error. This isn't a bug—it's the core message. Your future requires explicit vision and commitment.
- **Learning Through Failure**: The error-to-success journey mirrors real life. Understanding what's needed is part of the process.
- **Authentic Achievement**: When you finally boot your future with the right parameters, the success feels earned.

## Features

This project is a single, self-contained `index.html` file, but it's packed with features to create a compelling and shareable experience.

- **Realistic Terminal Interface**: A CLI-like environment simulated entirely in your browser, featuring a blinking cursor, colored output, and a familiar prompt (`$`).

- **Intro Animation**: When you first visit the page, an automated demo plays showing someone attempting to boot their future incorrectly:
    - Auto-types `./future.sh` with realistic timing
    - Shows the error message and usage instructions
    - Teaches new users what they need to do
    - Takes about 3 seconds total, creating anticipation
    - Can be skipped by pressing ESC
    
- **The Boot Command**: The core of the experience. Users must provide two critical flags to boot their future:
    
    - `--vision`: A string describing the future you want to build.
        
    - `--commit`: A string describing the first, concrete action you will take.
        
- **Interactive Mode (Training Mode)**: For those new to CLI, there's a guided mode that teaches the proper command structure:
    - Accessed via `./future.sh --interactive`
    - Walks users through the process step-by-step
    - Uses interactive Yes/No prompts with arrow key navigation
    - Shows the correct command format at the end
    - Generated images include a "TRAINING MODE" watermark
    - Designed as a learning tool, not a replacement for the real experience
        
- **Extended Options (NEW)**: Deepen your commitment with optional flags that enrich the boot sequence:
    - `--when`: Set a deadline for starting your commitment
    - `--first-step`: Define the very first micro-action you'll take
    - `--why`: Clarify your personal motivation
    - `--milestone`: Set an intermediate goal to track progress
    - When used, these options create a rich, tree-structured output showing your complete action plan
        
- **Internationalization**: Full multilingual support with manual control:
    - Detects language at page load using multiple methods (URL parameter, saved preference, browser language)
    - Supports URL parameters: `?lang=ja` or `?lang=en` for direct language selection
    - Saves language preference in browser for future visits
    - Manual language switching available with `lang [ja|en]` command
    - All messages, errors, help text, and social sharing are fully localized
    - Language setting persists throughout your session (no automatic switching based on input)
        
- **Shell Environment Commands**: Standard Unix commands for an authentic terminal experience:
    - `ls`: List files in the current directory
    - `cat <file>`: Display file contents with tab completion support:
        - Type `cat ` and press Tab to see available files
        - Type `cat R` and press Tab to auto-complete to `cat README.md`
        - `cat future.sh` shows "Permission denied" (the source is protected)
        - Supports `README.md` and `vision.txt` for reading
    - `clear`: Clear the terminal screen
    - `pwd`: Print working directory (`/home/future/commits`)
    - `whoami`: Display current user (`builder`)
    - `help`: Show available commands
        
- **Guide Command for Beginners**: A friendly hint system that helps lost users find their way:
    - After 3 consecutive errors (invalid commands or failed attempts), a hint appears: "💡 New here? Type 'guide' for beginner-friendly mode!"
    - Type `guide` (or `ガイド` in Japanese) to immediately start interactive mode
    - The hint appears as a separate, highlighted line to catch attention
    - Any successful command resets the error counter
    
- **Success & Failure States**:
    
    - If the command is run without the required flags, a `[FATAL]` error is displayed, along with a `USAGE` guide and a philosophical tip.
        
    - On successful execution, the app displays a log of your future being initiated, including the iconic `Daemonizing... Your future is now running in the background. Keep committing.` message.
        
- **Command History**: Just like a real terminal, you can navigate through your past commands using the `ArrowUp` and `ArrowDown` keys. The initial `./future.sh` command is pre-loaded into the history for convenience.
    
- **Perfectly Cropped Snapshot**: After a successful boot, a shareable image (`.png`) is automatically generated. The snapshot focuses only on your successful command and its output—no distracting error messages or failed attempts. This creates a clean, inspiring image perfect for sharing.
    
- **Interactive Share Prompt**: After successfully booting your future, you'll be asked "Would you like to share your future with others?"
    - Use arrow keys, number keys (1/2), or Y/N to make your choice
    - Selecting "Yes" reveals the share options
    - Selecting "No" returns you to the terminal with an encouraging message
    - 3-minute timeout automatically selects "No" if left idle
    
- **Easy Sharing**: When you choose to share, a share box appears with multiple options:
    
    1. **Download Image**: Save the generated snapshot directly to your device
    2. **Share on X**: Opens X (Twitter) with a pre-populated message
    3. **Share on Facebook**: Share your commitment on Facebook
    4. **Share on LinkedIn**: Post your professional future on LinkedIn
    
- **Share-Last Command**: Missed the share prompt? Use `share-last` to share your most recently booted future anytime during your session
        

## How to Use

### Standard Mode (Recommended)

1. Visit the website.
    
2. You will be greeted by an initial error message, prompting you for action.
    
3. At the prompt, type your command using the following format. You can use single (`'`) or double (`"`) quotes.
    
    ```
    ./future.sh --vision "a world where everyone can create" --commit "start learning to code today"
    ```
    
    Note: All variations work: `future`, `./future`, `future.sh`, and `./future.sh`. The `./future.sh` format follows proper shell conventions for executing local scripts.
    
4. Press `Enter`.
    
5. Watch your future boot.
    
6. Use the buttons at the top to download your personal snapshot and share it with the world.
    

### Interactive Mode (Training)

For those unfamiliar with command-line interfaces:

1. Type `./future.sh --interactive` or simply `guide`
    
2. Follow the guided prompts to enter your vision and commitment
    
3. The system will show you the proper command format
    
4. Use the interactive Yes/No prompt to confirm:
   - Arrow keys (↑/↓) to navigate
   - Number keys (1/2) for quick selection
   - Y/N shortcuts
   - Enter to confirm your choice
    
5. Watch your future boot
    
Note: Interactive mode is designed as a learning tool. Images generated in this mode will include a "TRAINING MODE" indicator. For the authentic experience and shareable images without watermarks, use the standard command format.

### Shell Environment Commands

To enhance the CLI experience, the following standard shell commands are also available:

```bash
ls          # List files in current directory
cat         # Display file contents (with tab completion)
clear       # Clear the terminal
pwd         # Print working directory
whoami      # Display current user
help        # Show help message
lang        # Switch language (ja/en)
guide       # Start beginner-friendly guided mode
share-last  # Share your last booted future
```

These commands make the environment feel more authentic and help users familiarize themselves with basic shell operations.

## Command Examples

```bash
# Standard execution (recommended)
./future.sh --vision "sustainable cities everywhere" --commit "bike to work this week"

# All command variations work
future --vision "change the world" --commit "start today"
./future --vision "better tomorrow" --commit "act now"
future.sh --vision "green planet" --commit "plant trees"

# With extended options for a detailed action plan
./future.sh --vision "become a web developer" --commit "complete online course" \
  --first-step "install VS Code today" --milestone "build portfolio site" \
  --why "create impactful solutions" --when "2025-01-01"

# Japanese input (use lang ja to switch interface)
lang ja
./future.sh --vision "持続可能な社会" --commit "今日から自転車通勤"

# Direct language selection via URL
# Visit: https://the-future.sh/?lang=ja

# Get help
./future.sh --help

# Interactive mode (for learning)
./future.sh --interactive

# Basic shell commands
ls              # See available files (future.sh, README.md, vision.txt)
cat README.md   # View this documentation (supports tab completion)
cat vision.txt  # See a motivational message
cat f[TAB]      # Auto-completes to future.sh (but shows permission denied)
pwd             # Show current directory (/home/future/commits)
whoami          # Display current user (builder)
help            # Get command help
clear           # Clean up the terminal
lang ja         # Switch to Japanese
lang en         # Switch to English
guide           # Start interactive mode (helpful for beginners)
share-last      # Share your previously booted future
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

## Recently Implemented Features

### ✅ Command Rename to future.sh (Complete)
The main command has been simplified from `boot_future.sh` to `future.sh`:
- Accepts multiple variations: `future`, `./future`, `future.sh`, `./future.sh`
- All references throughout the interface have been updated
- File system shows `future.sh` when using `ls` command
- Maintains backward compatibility in documentation

### ✅ Intro Animation (Complete)
An automated demonstration plays when you first visit the page:
- Shows someone attempting to run `./future.sh` without parameters
- Displays the error message to teach proper usage
- Uses realistic typing animation with varied speeds
- Creates anticipation and normalizes the "failure" experience
- Can be skipped by pressing ESC

### ✅ Enhanced Cat Command (Complete)
The `cat` command now includes tab completion and special file handling:
- Press Tab after `cat ` to see available files
- Auto-completes partial filenames (e.g., `cat R[TAB]` → `cat README.md`)
- `cat future.sh` shows "Permission denied" to maintain the mystery
- Full Unix-like tab completion behavior

### ✅ Interactive Mode (Complete)
The guided mode for CLI beginners is now live! Access it with `./future.sh --interactive` to get step-by-step prompts for creating your vision and commitment.

### ✅ Internationalization (Complete)
Full multilingual support with manual control:
- Language detected at page load (URL parameter → saved preference → browser language)
- Direct language selection via URL: `?lang=ja` or `?lang=en`
- Manual switching with `lang [ja|en]` command
- Language preference saved for future visits
- All messages, errors, help text, and social sharing are fully localized
- Language setting persists throughout your session (no auto-switching)

### ✅ Extended Options (Complete)
The optional flags for deeper commitment are now available:

```bash
# Core (Required)
--vision "your future vision"
--commit "your immediate action"

# Extended (Optional)
--when "2024-12-31"              # Deadline for starting
--first-step "download Python"    # The very first micro-action  
--why "for my children"          # Personal motivation
--milestone "app launch in 3mo"  # Intermediate goal
```

When you use extended options, the output becomes richer:

```
# Minimal (vision + commit only)
Future process started successfully. (PID: 20241225)
Daemonizing... Your future is now running in the background. Keep committing.

# Detailed (with optional flags)
Future process started successfully. (PID: 20241225)
├── First checkpoint: Download Python by tonight
├── Milestone: App launch in 3 months
├── Driven by: For my children
└── Deadline: 2024-12-31
Daemonizing... Your future is now running in the background. Keep committing.
```

### ✅ Share Prompt System (Complete)
An interactive sharing experience that respects user choice:
- After booting your future, you're asked if you want to share
- Interactive Yes/No selection with keyboard navigation
- `share-last` command to share later if you change your mind
- Clean, focused screenshots without error messages

### ✅ Guide Command & Hint System (Complete)
Helpful guidance for new users without being intrusive:
- After 3 consecutive errors, a friendly hint appears
- `guide` command (or `ガイド` in Japanese) starts interactive mode
- Works with any type of error (invalid commands or missing flags)
- Maintains the philosophy of "earning" the solution through effort

### ✅ Enhanced Interactive Prompts (Complete)
All confirmations now use the same intuitive interface:
- Arrow keys for navigation
- Number keys (1/2) for quick selection
- Y/N keyboard shortcuts
- Visual feedback with selection markers

## Future Development

### 📋 Planned Features

- **Command history persistence**: Remember your past futures across sessions
- **Achievement system**: Unlock easter eggs for power users
- **Export formats**: Generate wallpapers, calendar events, or reminder cards
- **Community futures**: Anonymously share and discover inspiring vision/commit pairs
- **AI-powered vision suggestions**: For those who have actions but need help seeing the bigger picture

### Implementation Timeline

1. **Phase 1**: ✅ Interactive mode (Complete)
2. **Phase 2**: ✅ Internationalization (Complete)
3. **Phase 3**: ✅ Extended options system (Complete)
4. **Phase 4**: Community features and additional export formats (Planned)

> **Note**: We're continuously evaluating new features based on community feedback while maintaining the core philosophy that "futures require deliberate effort."

What's your first commit?