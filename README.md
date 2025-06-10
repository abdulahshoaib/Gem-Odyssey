# Gem Odyssey Game Documentation

## Table of Contents
1. [Overview](#overview)
2. [System Requirements](#system-requirements)
3. [Installation & Setup](#installation--setup)
4. [Game Features](#game-features)
5. [How to Play](#how-to-play)
6. [Code Architecture](#code-architecture)
7. [Game Mechanics](#game-mechanics)
8. [Technical Implementation](#technical-implementation)
9. [Assets & Resources](#assets--resources)
10. [Troubleshooting](#troubleshooting)

## Overview

Gem Odyssey is a classic match-3 puzzle game developed in C++ using the SFML (Simple and Fast Multimedia Library) framework. Players swap adjacent gems on an 8×8 grid to create matches of three or more identical gems, earning points while racing against time.

### Key Features
- **Classic Match-3 Gameplay**: Swap gems to create horizontal or vertical matches
- **Special Gem System**: Create powerful gems with 4+ matches
- **Destroyer Mechanics**: Clear entire rows and columns with special destroyer gems
- **Combo System**: Chain matches for higher scores
- **Time-Based Challenge**: Race against a countdown timer
- **Intuitive Menu System**: Navigate through game options easily

## System Requirements

### Minimum Requirements
- **Operating System**: Windows 10/11, macOS 10.15+, or Linux Ubuntu 18.04+
- **Processor**: Intel Core i3 or AMD equivalent
- **Memory**: 2 GB RAM
- **Graphics**: DirectX 9.0c compatible
- **Storage**: 100 MB available space

### Dependencies
- **SFML Library**: Version 2.5 or higher
- **C++ Compiler**: Supporting C++11 standard or later
- **Graphics Support**: OpenGL 1.1 compatible graphics card

## Installation & Setup

1. **Install SFML Library**
   ```bash
   # Ubuntu/Debian
   sudo apt-get install libsfml-dev

   # macOS (using Homebrew)
   brew install sfml

   # Windows - Download from https://www.sfml-dev.org/
   ```

2. **Compile the Game**
   ```bash
   g++ -o gem_odyssey main.cpp -lsfml-graphics -lsfml-window -lsfml-system
   ```

3. **Run the Game**
   ```bash
   ./gem_odyssey
   ```

## Game Features

### Core Gameplay
- **8×8 Gem Grid**: Strategic gameplay area with 64 gem positions
- **6 Gem Types**: Different colored gems with unique visual designs
- **Swap Mechanics**: Exchange adjacent gems (horizontal/vertical only)
- **Match Detection**: Automatic detection of 3+ gem matches
- **Gravity System**: Gems fall to fill empty spaces after matches

### Scoring System
- **Basic Match (3 gems)**: 100 points
- **Extended Match (4 gems)**: 200 points + Special Gem
- **Large Match (5+ gems)**: 300+ points + Destroyer Gem
- **Combo Multiplier**: Consecutive matches increase score multiplier

### Special Elements
- **Special Gems**: Created from 4-gem matches, clear surrounding gems
- **Destroyer Gems**: Generated from 5+ matches, eliminate entire rows/columns
- **Chain Reactions**: Cascading matches from falling gems

## How to Play

### Basic Controls
- **Arrow Keys**: Navigate menu options and move gem selector
- **Enter Key**: Select menu items or choose gems for swapping
- **ESC Key**: Return to previous menu or exit game

### Gameplay Steps
1. **Select First Gem**: Use arrow keys to highlight a gem, press Enter
2. **Select Adjacent Gem**: Move to an adjacent gem and press Enter to swap
3. **Create Matches**: Form lines of 3+ identical gems (horizontal/vertical)
4. **Score Points**: Matched gems disappear and new ones fall from above
5. **Beat the Clock**: Maximize your score before time runs out

### Strategy Tips
- Look for potential chain reactions when gems fall
- Create special gems by matching 4+ gems
- Use destroyer gems strategically to clear difficult areas
- Plan moves to set up multiple matches in sequence

## Code Architecture

### Main Components

#### `main()` Function
**Purpose**: Entry point and menu system controller
- Initializes SFML window (800×600 resolution)
- Manages main menu navigation
- Handles user input for menu selection
- Routes to appropriate game functions

```cpp
// Key responsibilities:
- Window creation and configuration
- Menu button rendering and interaction
- Event handling for menu navigation
- Function delegation based on user selection
```

#### `game()` Function
**Purpose**: Core gameplay loop and game state management
- Sets up game window and UI elements
- Manages gem grid state (8×8 matrix)
- Handles player input for gem swapping
- Updates game timer and score display
- Renders all game elements

```cpp
// Key responsibilities:
- Game board initialization
- Real-time input processing
- Game state updates
- Rendering pipeline management
- Timer and score tracking
```

#### `infoMenu()` Function
**Purpose**: Displays game instructions and controls
- Shows how-to-play information
- Explains control scheme
- Provides gameplay tips

#### `credMenu()` Function
**Purpose**: Displays developer credits and acknowledgments
- Shows development team information
- Lists contributor details
- Displays version information

### Support Functions

#### UI Functions
- `createButton()`: Generates interactive menu buttons
- `loadTexture()`: Handles texture loading and error checking

#### Game Logic Functions
- `Check()`: Main match detection and processing
- `CheckDestroyer()`: Handles destroyer gem mechanics
- `CheckHor()/CheckVer()`: Directional match checking
- `BlastHor()/BlastVer()`: Row/column clearing functions

## Game Mechanics

### Match Detection Algorithm

The game uses a comprehensive matching system that scans the grid for valid matches:

1. **Horizontal Scanning**: Checks each row for consecutive identical gems
2. **Vertical Scanning**: Checks each column for consecutive identical gems
3. **Match Validation**: Confirms matches of 3+ gems
4. **Special Gem Creation**: Generates special gems for 4+ matches

### Special Gem System

#### Special Gem Creation
- **Trigger**: Match exactly 4 gems in a line
- **Effect**: Creates a special gem (value > 9) at match location
- **Activation**: When matched, clears 3×3 area around the gem

#### Destroyer Gem Creation
- **Trigger**: Match 5 or more gems in a line
- **Effect**: Creates a destroyer gem with enhanced clearing power
- **Activation**: Clears entire row and column when matched

### Combo System

The combo system rewards consecutive matches:

1. **Chain Detection**: Identifies matches caused by falling gems
2. **Multiplier Calculation**: Increases score based on combo length
3. **Cascade Processing**: Continues until no new matches form

### Physics Simulation

#### Gravity System
- Gems fall downward to fill empty spaces
- Multiple gems can fall simultaneously
- Falling continues until all spaces are filled

#### Gem Generation
- New gems spawn at the top of columns
- Random gem types maintain game balance
- Ensures no immediate matches in new gems

## Technical Implementation

### Data Structures

#### Gem Grid Representation
```cpp
int grid[8][8];  // Main game board
// Values 1-6: Regular gem types
// Values 10+: Special gems
// Value 0: Empty space
```

#### Game State Variables
```cpp
int score;           // Current player score
int timeRemaining;   // Countdown timer
int selectedX, selectedY;  // Currently selected gem position
bool gameActive;     // Game state flag
```

### Rendering Pipeline

1. **Background Rendering**: Draws game background texture
2. **Grid Rendering**: Displays gems based on grid values
3. **UI Rendering**: Shows score, timer, and game elements
4. **Selection Highlighting**: Indicates currently selected gem
5. **Buffer Swapping**: Updates display with new frame

### Input Processing

#### Event Handling Loop
```cpp
while (window.pollEvent(event)) {
    switch (event.type) {
        case sf::Event::KeyPressed:
            // Process keyboard input
        case sf::Event::Closed:
            // Handle window close
    }
}
```

#### State-Based Input
- **Menu State**: Arrow keys for navigation, Enter for selection
- **Game State**: Arrow keys for gem selection, Enter for swapping
- **Pause State**: ESC key handling for menu return

### Memory Management

#### Texture Loading
- Centralized texture loading with error checking
- Resource cleanup on program exit
- Efficient texture reuse for identical gems

#### Performance Optimization
- Minimal object creation during gameplay
- Efficient match detection algorithms
- Optimized rendering calls

## Assets & Resources

### Required Files

#### Graphics Assets
```
/assets/images/
├── background.png       # Main menu background
├── game_background.png  # Game screen background
├── gem1.png            # Gem type 1 sprite
├── gem2.png            # Gem type 2 sprite
├── gem3.png            # Gem type 3 sprite
├── gem4.png            # Gem type 4 sprite
├── gem5.png            # Gem type 5 sprite
├── gem6.png            # Gem type 6 sprite
├── special_gem.png     # Special gem sprite
└── destroyer_gem.png   # Destroyer gem sprite
```

#### Font Assets
```
/assets/fonts/
└── game_font.ttf       # Main game font
```

### Asset Specifications

#### Image Requirements
- **Format**: PNG with transparency support
- **Gem Size**: 64×64 pixels recommended
- **Background Size**: 800×600 pixels (window size)
- **Color Depth**: 32-bit RGBA

#### Font Requirements
- **Format**: TrueType Font (.ttf)
- **Size**: Scalable vector font
- **Character Set**: Basic ASCII (32-126)

## Troubleshooting

### Common Issues

#### Compilation Errors
**Problem**: SFML headers not found
**Solution**:
```bash
# Ensure SFML is properly installed
pkg-config --libs sfml-graphics sfml-window sfml-system
```

**Problem**: Linking errors
**Solution**:
```bash
# Include all required SFML libraries
g++ -o gem_odyssey main.cpp -lsfml-graphics -lsfml-window -lsfml-system -lsfml-audio
```

#### Runtime Errors
**Problem**: Font loading failure
**Solution**:
- Verify font file exists in correct path
- Check file permissions
- Ensure font file is not corrupted

**Problem**: Image loading failure
**Solution**:
- Confirm image files are in assets directory
- Verify image format compatibility
- Check file path case sensitivity (Linux/macOS)

#### Performance Issues
**Problem**: Low frame rate
**Solution**:
- Reduce window size if necessary
- Optimize graphics driver settings
- Close other resource-intensive applications

**Problem**: Input lag
**Solution**:
- Check for high CPU usage
- Verify proper event handling implementation
- Consider VSync settings

### Debug Mode

Enable debug output by adding compiler flag:
```bash
g++ -DDEBUG -o gem_odyssey main.cpp -lsfml-graphics -lsfml-window -lsfml-system
```
