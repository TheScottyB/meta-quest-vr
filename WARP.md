# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

This is a Unity VR project targeting Meta Quest 2/3 headsets with:
- **Unity Version**: 2023.3.0a15 (alpha - consider upgrading to 2022.3 LTS for stability)
- **Platform**: Android (Meta Quest)
- **Rendering**: HDRP (High Definition Render Pipeline) v17.0.1
- **Meta XR SDK**: v59.0.0
- **Key Features**: Hand tracking, controller input, ML-Agents v2.0.1, XR Interaction Toolkit
- **Product Name**: "My Sandbox" (see ProjectSettings.asset)
- **Company**: DefaultCompany
- **Bundle Version**: 0.1.0
- **Min Android SDK**: 23, Target: Auto (33+)

## Essential Commands

### Device Management
```bash
# Verify Quest is connected and recognized
adb devices

# Install/launch built APK manually
adb install -r path/to/build.apk
adb shell am start -n com.YourCompany.YourApp/com.unity3d.player.UnityPlayerActivity
```

### Unity Editor (via Unity Hub)
```bash
# Open project in Unity Hub
open -a "Unity Hub"

# Note: No direct CLI commands for Unity builds on macOS
# All builds must be done through Unity Editor: File → Build Settings → Build and Run
```

### Development Workflow
1. **Testing in Editor**: Use Meta XR Simulator (Meta → Meta XR Simulator → Activate, then press Play)
   - WASD: Move head
   - Mouse: Look around
   - Spacebar: Center view
2. **Device Testing**: Build full APK via Unity Editor (File → Build Settings → Build and Run)
3. **First build**: Allow 10-20 minutes for initial Gradle build

### Checking Unity Version
```bash
# Check Unity version from ProjectVersion.txt
cat ProjectSettings/ProjectVersion.txt
```

### Debugging on Device
```bash
# View real-time logs from Quest while app is running
adb logcat -s Unity ActivityManager PackageManager dalvikvm DEBUG

# Clear logcat before testing
adb logcat -c

# Filter Unity-specific logs
adb logcat | grep Unity
```

## Architecture

### Unity Package Dependencies
Core packages (see `Packages/manifest.json`):
- **Meta XR SDK** (`com.meta.xr.sdk.all`): Complete Meta Quest development stack
- **XR Systems**: `com.unity.xr.management`, `com.unity.xr.oculus`, `com.unity.xr.openxr`, `com.unity.xr.hands`
- **Rendering**: `com.unity.render-pipelines.high-definition` (HDRP)
- **ML-Agents**: `com.unity.ml-agents` v2.0.1 for AI/machine learning integration

### Project Structure
```
Assets/
├── Resources/         # Oculus runtime settings (OculusRuntimeSettings.asset, OVRPlatformToolSettings)
├── Settings/          # HDRP render pipeline assets and quality settings
└── XR/                # XR-specific configuration (OpenXR settings)

ProjectSettings/       # Unity project configuration
├── ProjectSettings.asset      # Android build settings, API levels
├── XRSettings.asset           # XR plugin configuration
└── HDRPProjectSettings.asset  # HDRP quality/performance settings
```

### Android Build Configuration
- **Build System**: Gradle
- **Minimum API Level**: 23
- **Target API Level**: 33+
- **Texture Compression**: ASTC
- **Stereo Rendering**: Single Pass Instanced (configured in XR Plugin Management → Oculus settings)
- **Hand Tracking**: Controllers and Hands mode enabled

## Critical macOS Limitations

⚠️ **macOS does NOT support Oculus Link** - Cannot use Play Mode with Quest connected
- Use **Meta XR Simulator** for rapid editor iteration
- Device testing requires full APK builds each time

## Performance Considerations

### HDRP is Heavy for Mobile VR
- Quest 2: Target 72Hz, max 1832x1920 per eye
- Quest 3: Target 90-120Hz, max 2064x2208 per eye
- **Consider switching to URP** (Universal Render Pipeline) if performance issues arise
- Disable HDR and reduce post-processing effects for mobile VR optimization

### Recommended Workflow
1. Use Meta → Tools → Project Setup Tool → "Fix All" + "Apply All" for Meta-recommended settings
2. Test builds on device regularly to catch performance issues early
3. Monitor frame rate targets (72Hz minimum for Quest 2)

## Development Tools Setup

### Prerequisites
- Unity Hub + Unity 2022.3 LTS (with Android Build Support, SDK/NDK, OpenJDK)
- Meta Quest Developer Hub (MQDH) - macOS version
- Android Platform Tools: `brew install --cask android-platform-tools`
- Meta Developer Account with Developer Mode enabled on Quest device

### Initial Project Setup
1. Enable Developer Mode on Quest (via Meta Quest mobile app)
2. Connect Quest via USB-C and approve USB debugging prompt in headset
3. In Unity: Edit → Project Settings → XR Plug-in Management → Android tab → Enable Oculus/Meta XR
4. Configure: Stereo Rendering Mode = Single Pass Instanced, Hand Tracking Support = Controllers and Hands

## Current Project State

⚠️ **This is a fresh Unity project** - No game code has been written yet
- No C# scripts in Assets/
- No Unity scenes (.unity files) created
- Contains only Meta XR SDK setup and HDRP configuration files
- Assets contain only Meta XR resources and configuration

### When Creating New Scripts
- Place gameplay scripts in `Assets/Scripts/`
- Place editor scripts in `Assets/Editor/`
- Follow C# naming conventions (PascalCase for classes and methods)
- Use Unity's component-based architecture
- Leverage Meta XR SDK OVR components (OVRCameraRig, OVRHand, OVRGrabber, etc.)

### When Creating Scenes
- Place scenes in `Assets/Scenes/`
- Always include an OVRCameraRig prefab (from Meta XR SDK) for VR camera setup
- Configure lighting for HDRP (avoid legacy lighting)
- Test both controller and hand tracking modes

## Known Issues

- **Unity Version**: Project uses alpha version (2023.3.0a15) - upgrade to 2022.3 LTS recommended for production
- **HDRP Performance**: May need to migrate to URP for better Quest performance
- **macOS Build Limitations**: Gradle builds on first run can take 10-20 minutes; subsequent builds are faster
