# the-future.sh

> A tiny CLI simulator to boot your future.

`the-future.sh` is not just a tool; it's an interactive web experience designed to inspire action. It frames the abstract idea of building a future in the familiar language of a command-line interface, reminding us that a vision, without a commit, is just a dream.

_(You can replace this with a link to your own screenshot)_

## Features

This project is a single, self-contained `index.html` file, but it's packed with features to create a compelling and shareable experience.

- **Realistic Terminal Interface**: A CLI-like environment simulated entirely in your browser, featuring a blinking cursor, colored output, and a familiar prompt (`$`).
    
- **The Boot Command**: The core of the experience. Users must provide two critical flags to boot their future:
    
    - `--vision`: A string describing the future you want to build.
        
    - `--commit`: A string describing the first, concrete action you will take.
        
- **Success & Failure States**:
    
    - If the command is run without the required flags, a `[FATAL]` error is displayed, along with a `USAGE` guide and a philosophical tip.
        
    - On successful execution, the app displays a log of your future being initiated, including the iconic `Daemonizing... Your future is now running in the background. Keep committing.` message.
        
- **Command History**: Just like a real terminal, you can navigate through your past commands using the `ArrowUp` and `ArrowDown` keys. The initial `./boot_future.sh` command is pre-loaded into the history for convenience.
    
- **Perfectly Cropped Snapshot**: After a successful boot, a shareable image (`.png`) is automatically generated. This snapshot is intelligently cropped to include the entire story of your session—from the initial "error" message to your successful command execution—with no wasted space.
    
- **Easy Sharing**: A share box appears at the top of the screen with two options:
    
    1. **Download Image**: Save the generated snapshot directly to your device.
        
    2. **Share on X**: Opens the X (Twitter) intent window with a pre-populated template, ready for you to share your commitment. The button order is intentionally designed to encourage downloading the image first.
        

## How to Use

1. Visit the website.
    
2. You will be greeted by an initial error message, prompting you for action.
    
3. At the prompt, type your command using the following format. You can use single (`'`) or double (`"`) quotes.
    
    ```
    ./boot_future.sh --vision "a world where everyone can create" --commit "start learning to code today"
    ```
    
4. Press `Enter`.
    
5. Watch your future boot.
    
6. Use the buttons at the top to download your personal snapshot and share it with the world.
    

## The Philosophy

The future isn't something you wait for; it's something you build, one commit at a time. This project is a small tribute to that idea. It's a reminder that the most ambitious vision begins with a single, tangible action.

What's your first commit?

## Tech Stack

- **Vanilla HTML, CSS, JavaScript**: No frameworks. Just the fundamentals.
    
- [**Typed.js**](https://github.com/mattboldt/typed.js/ "null"): For the typing animation effect.
    
- [**html2canvas**](https://github.com/niklasvh/html2canvas "null"): For generating the image snapshot from DOM elements.
    
- **Deployment**: Hosted on [Vercel](https://vercel.com/ "null").