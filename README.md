# Meta Quest VR Project

A Unity VR project for Meta Quest headsets with hand tracking, ML-Agents, and HDRP rendering.

## Project Overview

- **Unity Version**: 2023.3.0a15 (recommend upgrading to 2022.3 LTS for stability)
- **Platform**: Meta Quest 2/3
- **Rendering**: High Definition Render Pipeline (HDRP)
- **Meta XR SDK**: v59.0.0
- **Features**:
  - Hand tracking support
  - Controller input (Oculus Touch)
  - Unity ML-Agents v2.0.1
  - XR Interaction Toolkit
  - OpenXR support

## Prerequisites

### Software Required
- **Unity Hub** (latest version) - [Download](https://unity.com/download)
- **Unity 2022.3 LTS** with modules:
  - Android Build Support
  - Android SDK & NDK Tools
  - OpenJDK
  - Android Platform Tools
- **Meta Quest Developer Hub** - [Download](https://developer.oculus.com/downloads/package/oculus-developer-hub-mac/)
- **Android Platform Tools** (adb) - Install via `brew install --cask android-platform-tools`
- **Meta Developer Account** - [Create account](https://developer.oculus.com/)

### Hardware Required
- Meta Quest 2 or Quest 3 headset
- USB-C cable (data + power capable)
- Mac with Apple Silicon or Intel processor
- Minimum 8GB RAM (16GB+ recommended)

## Setup Instructions

### 1. Install Development Tools

```bash
# Install Android Platform Tools
brew install --cask android-platform-tools

# Verify adb installation
adb version
```

### 2. Install Unity Hub and Unity Editor

1. Download Unity Hub from https://unity.com/download
2. Install Unity Hub to Applications folder
3. Launch Unity Hub and sign in (create account if needed)
4. Install Unity 2022.3 LTS:
   - Go to **Installs** tab
   - Click **Install Editor**
   - Select **2022.3 LTS**
   - Add modules:
     - ✅ Android Build Support
     - ✅ Android SDK & NDK Tools
     - ✅ OpenJDK

### 3. Install Meta Quest Developer Hub

1. Download MQDH from https://developer.oculus.com/downloads/
2. Install to Applications folder
3. Launch and sign in with Meta Developer account

### 4. Enable Developer Mode on Quest

1. Install **Meta Quest Mobile App** on your phone
2. Pair your Quest headset
3. Go to **Settings** → **Developer**
4. Enable **Developer Mode**

### 5. Open Project in Unity

```bash
# Navigate to project directory
cd ~/Workspace/meta-quest-vr

# Open in Unity Hub
open -a "Unity Hub"
```

1. In Unity Hub, click **Open** → **Add project from disk**
2. Select the `meta-quest-vr` folder
3. Unity will detect the project and open it
4. If prompted about Unity version, choose to upgrade or continue

## Build Configuration

### Android Build Setup

1. Go to **File** → **Build Settings**
2. Select **Android** platform
3. Click **Switch Platform**
4. Configure settings:
   - **Texture Compression**: ASTC
   - **Build System**: Gradle
   - **Minimum API Level**: 23
   - **Target API Level**: 33+

### XR Plugin Management

1. Go to **Edit** → **Project Settings** → **XR Plug-in Management**
2. Select **Android** tab (📱 icon)
3. Enable:
   - ✅ **Oculus** (or Meta XR)
   - ✅ **OpenXR** (optional)
4. Under **Oculus** settings:
   - **Stereo Rendering Mode**: Single Pass Instanced
   - **Hand Tracking Support**: Controllers and Hands

### Project Optimization (Meta Project Setup Tool)

1. Go to **Meta** → **Tools** → **Project Setup Tool**
2. Click **Fix All** for required items
3. Click **Apply All** for recommended items

## Building and Deploying

### Connect Quest Device

```bash
# Connect Quest via USB-C cable
# Put on headset and allow USB debugging when prompted

# Verify device connection
adb devices
```

### Build APK

1. Go to **File** → **Build Settings**
2. Click **Build and Run**
3. Choose save location for APK
4. Wait for build to complete (first build may take 10-20 minutes)
5. App will automatically launch on Quest

## Testing

### Meta XR Simulator (Editor Testing)

1. Install **Meta XR Simulator** package via Package Manager
2. Go to **Meta** → **Meta XR Simulator** → **Activate**
3. Press **Play** in Unity Editor
4. Use keyboard/mouse to simulate VR:
   - **WASD**: Move head
   - **Mouse**: Look around
   - **Spacebar**: Center view

### Device Testing

- Build and deploy full APK to Quest device
- Test both **hand tracking** and **controller** modes
- Monitor performance (target 72Hz for Quest 2, 90Hz+ for Quest 3)

## Known Issues & Limitations

### macOS Limitations
- ❌ **No Oculus Link support** (Windows only)
- ❌ Cannot use Play Mode with Quest (requires full APK build each time)
- ✅ Use Meta XR Simulator for rapid iteration in editor

### Performance Considerations
- **HDRP is heavy for mobile VR** - May need to switch to URP (Universal Render Pipeline) for better performance
- Quest 2: Target 72Hz, max resolution 1832x1920 per eye
- Quest 3: Target 90-120Hz, max resolution 2064x2208 per eye
- Disable HDR and reduce post-processing effects for better performance

### Unity Version
- Project currently uses Unity 2023.3.0a15 (alpha - unstable)
- **Recommended**: Upgrade to Unity 2022.3 LTS for production

## Project Structure

```
meta-quest-vr/
├── Assets/
│   ├── Resources/           # OculusRuntimeSettings, OVRPlatformToolSettings
│   ├── Settings/            # HDRP settings and render pipeline assets
│   ├── XR/                  # XR-specific settings
│   └── [Future: Scenes, Scripts, Prefabs]
├── Packages/
│   └── manifest.json        # Package dependencies
├── ProjectSettings/         # Unity project configuration
└── README.md
```

## Dependencies

Key packages (see `Packages/manifest.json` for full list):

- `com.meta.xr.sdk.all` v59.0.0
- `com.unity.render-pipelines.high-definition` v17.0.1
- `com.unity.ml-agents` v2.0.1
- `com.unity.xr.management` v4.4.0
- `com.unity.xr.oculus` v4.1.2
- `com.unity.xr.openxr` v1.9.1
- `com.unity.xr.hands` v1.3.0

## Resources

### Documentation
- [Meta XR Unity Documentation](https://developers.meta.com/horizon/documentation/unity/)
- [Unity XR Interaction Toolkit](https://docs.unity3d.com/Packages/com.unity.xr.interaction.toolkit@latest)
- [Meta Quest Developer Center](https://developer.oculus.com/)

### Samples
- Meta XR SDK includes sample scenes - import from Package Manager
- Unity XR Interaction Toolkit samples

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test on Quest device
5. Submit a pull request

## License

[Add your license here]

## Support

For issues or questions:
- GitHub Issues: [Repository issues page]
- Meta Developer Forums: https://communityforums.atmeta.com/t5/Developer/ct-p/developer
- Unity Forums: https://forum.unity.com/

---

**Last Updated**: January 16, 2026
**Repository**: https://github.com/TheScottyB/meta-quest-vr
