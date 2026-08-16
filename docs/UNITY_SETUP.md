# Unity Configuration Guide - Meta Quest VR

Step-by-step guide for configuring Unity for Meta Quest development.

## Phase 1: Android Build Configuration

### Step 1: Switch to Android Platform

1. Go to **File** → **Build Settings** (or press `Cmd+Shift+B`)
2. In the Platform list, select **Android**
3. Click **Switch Platform** button
   - Unity will re-import all assets for Android (may take 5-10 minutes)
4. Wait for the progress bar to complete

### Step 2: Configure Android Build Settings

In the Build Settings window:
- **Texture Compression**: ASTC
- **Build System**: Gradle
- Click **Player Settings** button (bottom left)

### Step 3: Configure Player Settings (Android Tab)

In Project Settings → Player → Android tab:

**Other Settings section:**
- **Package Name**: Change from default (e.g., `com.YourCompany.MetaQuestVR`)
- **Minimum API Level**: Android 6.0 'Marshmallow' (API level 23)
- **Target API Level**: Automatic (highest installed) or API level 33+
- **Scripting Backend**: IL2CPP
- **Target Architectures**: ✅ ARM64 only (uncheck ARMv7)

**XR Settings section:**
- This will be configured in XR Plugin Management (next phase)

## Phase 2: XR Plugin Management Configuration

### Step 1: Open XR Plugin Management

1. Go to **Edit** → **Project Settings**
2. Select **XR Plug-in Management** in left sidebar
3. You'll see tabs at top: **🖥️ PC/Mac/Linux** and **📱 Android**

### Step 2: Configure Android XR Settings

1. Click the **📱 Android** tab
2. Enable plugins:
   - ✅ **Oculus** (or **Meta XR**)
   - ✅ **OpenXR** (optional, but recommended for cross-platform)

### Step 3: Configure Oculus/Meta XR Settings

Click on **Oculus** in the left sidebar:
- **Stereo Rendering Mode**: Single Pass Instanced
- **V2 Signing** (Quest 2): Enabled
- **Low Overhead Mode**: Disabled
- **Optimize Buffer Discards**: Enabled
- **Phase Sync**: Disabled
- **Subsampled Layout**: Disabled

Under **Quest Features**:
- **Hand Tracking Support**: Controllers and Hands
- **Hand Tracking Frequency**: MAX
- **Target Devices**: Quest, Quest 2, Quest 3

## Phase 3: Meta Project Setup Tool

This tool automatically applies Meta's recommended settings.

### Run the Setup Tool

1. In Unity menu bar: **Meta** → **Tools** → **Project Setup Tool**
   - If you don't see "Meta" menu, the Meta XR SDK may need to be updated
2. In the Project Setup Tool window:
   - Review all items under **Required** tab
   - Click **Fix All** button
   - Review items under **Recommended** tab  
   - Click **Apply All** button
3. Close the window when done

## Phase 4: HDRP Optimization for Quest

HDRP is heavy for mobile VR. Apply these optimizations:

### Quality Settings

1. **Edit** → **Project Settings** → **Quality**
2. Select **HDRP** quality level
3. In **Assets/Settings/HDRP Performant.asset**:
   - **HDR**: Disabled
   - **MSAA**: Disabled (use TAA instead)
   - **Shadow Quality**: Low or Medium
   - **Shadow Distance**: 50-100 meters max
   - **Reflection Probes**: Reduce count and quality
   - **Screen Space Reflections**: Disabled
   - **Post Processing**: Disable expensive effects (AO, Motion Blur, DOF)

### Graphics Settings

1. **Edit** → **Project Settings** → **Graphics**
2. Verify HDRP render pipeline asset is assigned
3. In **Assets/Settings/HDRPDefaultResources/HDRenderPipelineAsset.asset**:
   - **Render Scale**: 0.8-1.0
   - **LOD Bias**: 1.0
   - **Maximum LOD Level**: 0

## Phase 5: Verify Configuration

### Check XR Settings

1. **Edit** → **Project Settings** → **XR Plug-in Management**
2. Verify Android tab shows Oculus enabled
3. Check for any warning icons

### Check Build Settings

1. **File** → **Build Settings**
2. Verify:
   - Platform: Android
   - Texture Compression: ASTC
   - Development Build: ✅ (for testing)
   - Script Debugging: ✅ (optional, for debugging)

### Check Console for Errors

1. Open **Console** window (Cmd+Shift+C)
2. Look for any red errors
3. Address critical errors before proceeding

## Phase 6: Create VR Scene

See `VR_SCENE_SETUP.md` for detailed scene creation steps.

## Common Issues

### Issue: "Meta" Menu Not Visible
**Solution**: 
- Verify Meta XR SDK is installed: **Window** → **Package Manager** → **Packages: In Project**
- Look for `com.meta.xr.sdk.all`
- If missing, add from Package Manager

### Issue: Android Module Not Installed
**Solution**:
- Open Unity Hub
- Go to **Installs** tab
- Click gear icon next to Unity version
- Select **Add Modules**
- Enable **Android Build Support** with all sub-modules

### Issue: Stereo Rendering Not Working
**Solution**:
- Ensure XR Plugin Management is configured for Android tab (not Desktop)
- Verify Oculus/Meta XR plugin is enabled
- Check that "Initialize XR on Startup" is checked

### Issue: Build Fails with Gradle Errors
**Solution**:
- First build always takes 10-20 minutes
- Ensure Android SDK and NDK are installed via Unity Hub
- Check internet connection (Gradle downloads dependencies)
- Try **Build Settings** → **Player Settings** → **Android** → **Publishing Settings** → **Build** → **Custom Gradle Properties Template** (enable)

## Next Steps

After configuration is complete:
1. Create a basic VR scene with XR Origin
2. Test in Meta XR Simulator
3. Build APK and deploy to Quest device

---

**Reference**: This configuration is based on Unity 2022.3 LTS / 2023.3 with Meta XR SDK v59.0.0
