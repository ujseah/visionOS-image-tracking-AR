# Spatial Art Gallery

[![Platform](https://img.shields.io/badge/platform-visionOS-blue.svg)](https://developer.apple.com/visionos/)
[![Swift](https://img.shields.io/badge/Swift-6.0-orange.svg)](https://swift.org)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

A spatial computing experience for visionOS that transforms physical artwork into interactive 3D experiences with spatial audio narration.

## Overview

Spatial Art Gallery demonstrates advanced visionOS capabilities by combining image tracking, 3D model visualization, and spatial audio to create an immersive museum experience. When users point their device at a physical artwork, the app recognizes it and overlays interactive 3D content with synchronized audio narration.

### Key Features

- **Image Tracking**: Real-time recognition and tracking of physical artwork using ARKit's `ImageTrackingProvider`
- **3D Model Exploration**: Interactive 3D representations of artwork anchored to physical paintings
- **Spatial Audio Narration**: Contextual audio descriptions synchronized with the visual experience
- **Mixed Reality Interface**: Seamless blend of physical and digital content in shared space
- **Playback Controls**: Intuitive media controls for audio narration with play/pause, skip, and progress tracking

## Technical Stack

### Frameworks & APIs

- **RealityKit**: 3D rendering and entity management
- **ARKit**: Image tracking and world tracking capabilities
- **SwiftUI**: Modern declarative UI framework
- **AVFoundation**: Audio playback and control
- **QuickLook**: Native 3D model preview integration

### Architecture

The app follows a clean MVVM-inspired architecture:

- `AppModel`: Observable application state managing AR session, tracking providers, and scene graph
- `ImmersiveView`: Main spatial experience view with RealityKit content
- `ImmersiveAudioViewMode`: Audio playback management and control
- `PlaybackControlsView`: Reusable audio control interface

## Requirements

- **Platform**: visionOS 2.0 or later
- **Development**: Xcode 16.0+, Swift 6.0+
- **Hardware**: Apple Vision Pro

## Getting Started

### Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/spatial-art-gallery.git
cd spatial-art-gallery
```

2. Open the project in Xcode:
```bash
open IrishGirl.xcodeproj
```

3. Select the Vision Pro simulator or connected device as your build target

4. Build and run the project (⌘R)

### Setup

1. **Add Reference Images**: Place your reference images in an AR Resource Group named `ARImages`
   - Navigate to the Asset Catalog
   - Add physical dimensions of your target artwork
   - Ensure high-quality images for optimal tracking

2. **Add 3D Assets**: Include your 3D model files (`.usdz` or `.heic` format)
   - Place files in the app bundle
   - Update the resource name in `AppModel.handlePlayButtonTap()` if needed

3. **Add Audio Narration**: Include audio files in supported formats (`.m4a`, `.mp3`, etc.)
   - Place audio files in the app bundle
   - Update the audio filename in `ImmersiveView` initialization

## Usage

### Launching the Experience

1. Launch the app on Apple Vision Pro
2. Tap **"Enter Experience"** to open the immersive space
3. Point your device at the target artwork
4. Once tracked, the **"Explore in 3D"** button appears on the painting

### Interacting with Content

- **Explore in 3D**: Tap to reveal the 3D model and start audio narration
- **Playback Controls**: Use the control bar to pause, rewind (10s), or fast-forward (10s)
- **Progress Tracking**: Scrub through audio using the progress slider
- **Stop Experience**: Tap to dismiss the 3D content and return to tracking mode

## Project Structure

```
IrishGirl/
├── IrishGirlApp.swift              # App entry point and scene configuration
├── AppModel.swift                  # Core application state and AR session management
├── ImmersiveView.swift             # Main spatial experience with RealityKit content
├── ContentView.swift               # 2D window entry interface
├── ImmersiveAudioViewMode.swift    # Audio playback management
├── PlaybackControlsView.swift     # Reusable audio controls UI
├── ToggleImmersiveSpaceButton.swift # Custom button for entering/exiting immersive space
├── QuickLookView.swift             # QuickLook integration wrapper
└── Assets/
    ├── ARImages.arassets           # Reference images for tracking
    ├── TheIrishGirl3D.heic         # 3D model asset
    └── paintingNarration1.m4a      # Audio narration file
```

## Implementation Details

### Image Tracking

The app uses ARKit's `ImageTrackingProvider` to recognize and track physical artwork:

```swift
private let imageTracking = ImageTrackingProvider(
    referenceImages: ReferenceImage.loadReferenceImages(inGroupNamed: "ARImages")
)
```

When an image anchor is detected, a transparent plane entity is created and positioned at the artwork's location, serving as the parent for all attached UI and 3D content.

### 3D Content Anchoring

All interactive elements (buttons, controls, 3D models) are attached as children to the tracked plane entity, ensuring they move and rotate with the physical artwork as the user moves around.

### Audio Management

The `ImmersiveAudioViewMode` class handles:
- Audio playback state
- Progress tracking and scrubbing
- Skip forward/backward functionality
- Automatic cleanup on dismissal

## Customization

### Adding New Artwork

1. Add the reference image to the AR Resource Group
2. Create or obtain a 3D model (`.usdz` or `.heic`)
3. Record or source audio narration
4. Update the model name and audio filename in the code

### Styling

The app uses visionOS's glass background effect for UI elements:

```swift
.glassBackgroundEffect()
```

Customize colors, sizing, and positioning by modifying the attachment positions in `ImmersiveView`.

## Performance Considerations

- **Image Quality**: Higher resolution reference images improve tracking accuracy
- **Model Complexity**: Optimize 3D models for real-time rendering on Vision Pro
- **Audio Format**: Use compressed formats (M4A) to reduce bundle size
- **Entity Management**: Clean up entities when not in use to maintain performance

## Acknowledgments

- Built for Apple Vision Pro using visionOS SDK
- Reference artwork: "The Irish Girl" demonstration
