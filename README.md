# Neothesia

Neothesia is a cross-platform MIDI visualizer and video encoder built in Rust. It converts MIDI files into beautiful piano visualization videos with colorful falling notes on a virtual piano keyboard.

This fork includes a command-line tool for rendering MIDI files to video without requiring the interactive GUI application.

## What You Can Create

With this tool, you can:
- **Generate piano tutorial videos** from any MIDI file
- **Create YouTube piano videos** with professional-looking visualizations
- **Customize video appearance** with different resolutions, note labels, and keyboard settings
- **Export high-quality MP4 videos** with synchronized audio

## Quick Start Guide - Generate Your First Video

Here's a complete example from cloning the repository to generating your first video:

### Step 1: Install Prerequisites

**Install Rust** (if not already installed):
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
```

**Install FFmpeg**:
```bash
# macOS
brew install ffmpeg

# Ubuntu/Debian
sudo apt install ffmpeg libavcodec-dev libavformat-dev libavutil-dev libavfilter-dev libavdevice-dev
```

### Step 2: Clone and Build

```bash
# Clone the repository
git clone https://github.com/sulavtimsina/Neothesia.git
cd Neothesia

# Build the project (this will take a few minutes)
cargo build --release -p neothesia-cli
```

### Step 3: Get a MIDI File

Download a sample MIDI file or use your own:
```bash
# Example: Download "Ode to Joy" from BitMidi or any MIDI source
# Place it in the project directory
# For this example, let's assume you have: my_song.mid
```

### Step 4: (Optional) Download a Soundfont

For audio in your video, download a soundfont:
```bash
# Download FluidR3_GM soundfont
curl -L http://www.musescore.org/download/fluid-soundfont.tar.gz -o fluid.tar.gz
tar -xzf fluid.tar.gz
# This extracts FluidR3_GM.sf2
```

### Step 5: Generate Your Video

**With audio** (recommended):
```bash
./target/release/neothesia-cli my_song.mid output.mp4 --soundfont FluidR3_GM.sf2
```

**Without audio** (silent video):
```bash
./target/release/neothesia-cli my_song.mid output.mp4
```

**Complete example with all options**:
```bash
./target/release/neothesia-cli my_song.mid output.mp4 \
  --soundfont FluidR3_GM.sf2 \
  --width 1920 \
  --height 1080 \
  --keyboard-letters \
  --note-labels
```

### Step 6: Watch Your Video

```bash
# The output.mp4 file is now ready!
# Open it with your default video player
open output.mp4  # macOS
xdg-open output.mp4  # Linux
```

### Complete Example Session

```bash
# Start from scratch
cd ~
git clone https://github.com/sulavtimsina/Neothesia.git
cd Neothesia

# Build
cargo build --release -p neothesia-cli

# Download a soundfont
curl -L http://www.musescore.org/download/fluid-soundfont.tar.gz -o fluid.tar.gz
tar -xzf fluid.tar.gz

# Get a MIDI file (example: you have ode_to_joy.mid)
# Generate video
./target/release/neothesia-cli ode_to_joy.mid my_first_video.mp4 --soundfont FluidR3_GM.sf2

# Output will show:
# Encoding started:
#  Encoded 120 frames (2s, 15%) in 3s
#  Encoded 240 frames (4s, 30%) in 6s
#  ... (continues until 100%)

# Video is ready!
```

**Note**: Use `./target/release/neothesia-cli` (with `./`) not just `neothesia-cli` - the `./` tells your terminal where to find the program.

## Prerequisites

Before using this tool, you need to install:

### 1. **FFmpeg** (Required)
The video encoder requires FFmpeg libraries.

**macOS:**
```bash
brew install ffmpeg
```

**Ubuntu/Debian:**
```bash
sudo apt install ffmpeg libavcodec-dev libavformat-dev libavutil-dev libavfilter-dev libavdevice-dev
```

**Windows:**
Download from [ffmpeg.org](https://ffmpeg.org/download.html) and add to PATH.

### 2. **Rust** (Required for building)
Install from [rustup.rs](https://rustup.rs/):
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### 3. **Soundfont File** (Optional but recommended)
For better audio quality, download a SoundFont (.sf2) file:
- [FluidR3_GM.sf2](http://www.musescore.org/download/fluid-soundfont.tar.gz) (Good quality, ~140MB)
- [GeneralUser GS](https://schristiancollins.com/generaluser.php) (High quality, ~30MB)
- Any other SF2 soundfont file

## Building the Project

Clone and build the CLI tool:

```bash
# Clone the repository
git clone https://github.com/sulavtimsina/Neothesia.git
cd Neothesia

# Build the CLI tool (release mode for better performance)
cargo build --release -p neothesia-cli

# The binary will be at: target/release/neothesia-cli
```

## Basic Usage

### Minimal Command

```bash
./target/release/neothesia-cli input.mid output.mp4
```

This generates a 1920x1080 video without audio (unless you provide a soundfont).

### Recommended Command (with audio)

```bash
./target/release/neothesia-cli input.mid output.mp4 --soundfont FluidR3_GM.sf2
```

## Command Options

### Full Syntax

```bash
neothesia-cli <MIDI_FILE> <MP4_FILE> [OPTIONS]
```

### Available Options

| Option | Description | Default | Example |
|--------|-------------|---------|---------|
| `--soundfont <SF2_FILE>` | Path to soundfont file for audio generation | None (silent video) | `--soundfont FluidR3_GM.sf2` |
| `--width <PIXELS>` | Video width (must be even number) | 1920 | `--width 1280` |
| `--height <PIXELS>` | Video height (must be even number) | 1080 | `--height 720` |
| `--keyboard-letters` | Show note letters (C, D, E, etc.) on keyboard keys | Off | `--keyboard-letters` |
| `--note-labels` | Show note names on falling notes | Off | `--note-labels` |
| `--note-scroller` | Show scrolling list of upcoming notes at top | Off | `--note-scroller` |

## Command Examples

### 1. Basic HD Video with Audio
```bash
neothesia-cli song.mid output.mp4 --soundfont FluidR3_GM.sf2
```

### 2. 4K Video for YouTube
```bash
neothesia-cli song.mid output_4k.mp4 \
  --soundfont FluidR3_GM.sf2 \
  --width 3840 \
  --height 2160
```

### 3. Tutorial Video with All Labels
```bash
neothesia-cli tutorial.mid tutorial.mp4 \
  --soundfont FluidR3_GM.sf2 \
  --keyboard-letters \
  --note-labels \
  --note-scroller
```

### 4. 720p Video (smaller file size)
```bash
neothesia-cli song.mid output_720p.mp4 \
  --soundfont FluidR3_GM.sf2 \
  --width 1280 \
  --height 720
```

### 5. Silent Video (no soundfont)
```bash
neothesia-cli song.mid silent_output.mp4 \
  --width 1920 \
  --height 1080
```

### 6. Complete Tutorial Video
```bash
neothesia-cli lesson.mid lesson_video.mp4 \
  --soundfont GeneralUser.sf2 \
  --width 1920 \
  --height 1080 \
  --keyboard-letters \
  --note-labels \
  --note-scroller
```

## Where to Get MIDI Files

### Free MIDI Sources:
1. **MuseScore** - [musescore.com](https://musescore.com)
   - Huge library of user-uploaded sheet music
   - Download as MIDI format

2. **BitMidi** - [bitmidi.com](https://bitmidi.com)
   - Free MIDI file archive
   - No registration required

3. **FreeMidi.org** - [freemidi.org](https://freemidi.org)
   - Organized by genre and artist

4. **Classical Music MIDI** - [classicalarchives.com](https://www.classicalarchives.com)
   - Classical music collection

5. **Create Your Own**
   - Use DAW software (FL Studio, Ableton, Logic Pro)
   - Use music notation software (MuseScore, Finale, Sibelius)
   - Record MIDI from a keyboard/controller

### Tips for MIDI Files:
- Look for files with separate tracks for left and right hand
- Check that the file uses standard piano (not drums or other instruments on wrong channels)
- Test the file in a MIDI player before rendering to verify it sounds correct

## Expected Output

The tool will:
1. Load your MIDI file
2. Render each frame at 60 FPS
3. Generate synchronized audio (if soundfont provided)
4. Encode to MP4 format with H.264 video codec
5. Display progress: `Encoded X frames (Ys, Z%) in Ws`

**Rendering time**: Approximately 1-2 minutes of rendering time per 1 minute of MIDI playback (depends on your hardware).

## Troubleshooting

### "command not found: neothesia-cli"
You need to use the full path to the binary. Use `./target/release/neothesia-cli` instead of just `neothesia-cli`.

The `./` prefix tells the shell to look in the current directory structure for the program.

Alternatively, you can:
- Add to PATH temporarily: `export PATH="$PATH:$(pwd)/target/release"`
- Install globally: `cargo install --path neothesia-cli`

### "width and height must be a multiple of two"
Video dimensions must be even numbers. Use values like 1920, 1280, 720, not 1921, 1281, 721.

### FFmpeg linking errors
Make sure FFmpeg development libraries are installed (see Prerequisites).

### No audio in output video
Add the `--soundfont` option with a valid .sf2 file path.

### Video encoding is slow
- Use `--release` flag when building: `cargo build --release -p neothesia-cli`
- Lower resolution: `--width 1280 --height 720`
- Check CPU usage - rendering is CPU-intensive

## Development

This is a fork of the original [Neothesia](https://github.com/PolyMeilex/Neothesia) project. The original Neothesia is an interactive piano learning application. This fork focuses on command-line video generation.

Original project: [github.com/PolyMeilex/Neothesia](https://github.com/PolyMeilex/Neothesia)

## License

Same license as the original Neothesia project.

## Credits

- Original Neothesia project by PolyMeilex
- Built with [WGPU](https://wgpu.rs/) for GPU rendering
- Inspired by [Synthesia](https://www.synthesiagame.com/)
