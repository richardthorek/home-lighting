# Software Setup Guide - Falcon Player (FPP) & Home Assistant Integration

## Overview

This guide covers software installation and configuration for the Falcon Player (FPP) on Raspberry Pi, xLights for sequencing, and Home Assistant integration via MQTT.

**Time Required**:
- FPP Installation: 30 minutes
- FPP Configuration: 1 hour
- Home Assistant MQTT Setup: 1 hour
- xLights Basics: 2-4 hours (ongoing learning)

---

## Part 1: Falcon Player (FPP) Installation

### 1.1 Download FPP Image

**Official FPP Release**:
- Website: https://falconchristmas.github.io/FPP/
- Download page: https://github.com/FalconChristmas/fpp/releases

**Latest Version** (as of 2025-12): FPP v7.x or later

**Choose the Right Image**:
- **Raspberry Pi 4 / Pi 3B+**: `FPP-v7.x-Pi.img.zip`
- Verify SHA256 checksum after download (listed on release page)

### 1.2 Flash SD Card

**Tools**: 
- [Raspberry Pi Imager](https://www.raspberrypi.com/software/) (recommended)
- Or [Balena Etcher](https://www.balena.io/etcher/)
- Or `dd` command (Linux/Mac advanced users)

**Using Raspberry Pi Imager** (easiest):

1. **Launch Raspberry Pi Imager**
2. **Choose Device**: Select "Raspberry Pi 4" (or your model)
3. **Choose OS**: 
   - Click "Use custom"
   - Select downloaded FPP `.img.zip` file (no need to unzip)
4. **Choose Storage**: Select your MicroSD card (32GB+ recommended)
5. **Click "Next"**
6. **OS Customization** (optional but recommended):
   - Set hostname: `fpp` (or custom name)
   - Enable SSH: Yes (for remote access)
   - Set username/password: `fpp` / `<your-secure-password>`
   - Configure WiFi: (if using WiFi, but Ethernet recommended)
7. **Click "Yes" to apply customizations**
8. **Click "Yes" to proceed with flash** (will erase SD card)
9. **Wait for completion** (5-10 minutes)
10. **Click "Continue"** and remove SD card

**Alternative - Balena Etcher**:
1. Select FPP `.img.zip` file
2. Select SD card
3. Flash (no customization options)

### 1.3 First Boot

1. **Insert SD card** into Raspberry Pi
2. **Connect Ethernet** cable from Pi to router
3. **Connect power** (12V to PiCap, which powers Pi)
4. **Wait 60-90 seconds** for first boot (Pi ACT LED will blink)

**Finding FPP on Network**:

**Method 1: mDNS** (easiest)
- Open browser: `http://fpp.local`
- If custom hostname: `http://<hostname>.local`

**Method 2: Router DHCP List**
- Check your router's connected devices list
- Look for "fpp" or "raspberrypi"
- Note IP address, access via `http://<ip-address>`

**Method 3: Network Scanner**
- Use tool like Angry IP Scanner or `nmap`
- Scan local subnet: `nmap -sn 192.168.1.0/24`
- Identify Pi by MAC address (starts with `B8:27:EB`, `DC:A6:32`, or `E4:5F:01`)

**Success**: FPP web interface should load showing dashboard

---

## Part 2: FPP Initial Configuration

### 2.1 System Setup

**FPP Menu → Status/Control → System**

**Settings to Configure**:

1. **Host Settings**:
   - Host Name: `fpp` (or your choice)
   - Host Description: "Home LED Controller" (your choice)
   - Time Zone: Select your timezone
   - Audio Output: "HDMI" or "Disabled" (usually not needed for LEDs)

2. **Network Settings** (if not set during imaging):
   - Interface: `eth0` (Ethernet)
   - DHCP: Enabled (or set static IP if preferred)

3. **FPP Settings**:
   - Mode: **Standalone** (for single controller setup)
   - Blank between sequences: Enabled
   - Blank on startup: Enabled (or disabled if you want lights on at boot)

4. **MultiSync Settings** (ignore for single controller):
   - Blank

5. **Save Settings** and **Reboot** if prompted

### 2.2 Configure Channel Outputs

**FPP Menu → Input/Output Setup → Channel Outputs**

This is where you tell FPP about your LED hardware.

**For Falcon PiCap**:

1. **Enable PiCap Module**:
   - In "FPP Strings Attached" section
   - Enable: **Yes**
   - Type: `FPP PiCap`

2. **Configure Output #1**:
   - Enable: **Yes**
   - Start Channel: `1`
   - Pixel Count: `500` (example for your Zone 1)
   - Color Order: `RGB` (try `GRB` if colors are wrong)
   - Null Pixels: `0` (usually)
   - Group Count: `1` (for WS2811, set to `3` if using WS2811 strips that group 3 LEDs)
   - Reverse: No (unless you wired backwards)
   - Brightness: `100` (can reduce to 70-80% for power savings)
   - Gamma: `1.0` (default)
   - Description: `Zone 1 - Front Roofline`

3. **Configure Output #2** (if using):
   - Enable: **Yes**
   - Start Channel: `1501` (previous end channel + 1) = (500 × 3 channels + 1)
   - Pixel Count: `300` (example for Zone 2)
   - Color Order: `RGB`
   - Description: `Zone 2 - Side Eaves`

4. **Set Pixel Type**:
   - Under "Type" dropdown: Select `WS2811` (for 12V pixels) or `WS2812B` (for 5V)

5. **Save Settings**

**Channel Calculation**:
- Each RGB pixel uses **3 channels** (Red, Green, Blue)
- 500 pixels = 1500 channels
- Output 1: Channels 1-1500
- Output 2: Channels 1501-2400 (if 300 pixels)

### 2.3 Test Outputs

**FPP Menu → Status/Control → Display Testing**

**Test Mode Options**:

1. **Cycle Colors**:
   - Start Channel: `1`
   - End Channel: `1500` (for 500 pixels)
   - Click "Cycle Colors"
   - LEDs should cycle: Red → Green → Blue → White → Off

2. **RGB Fill**:
   - Set R/G/B sliders
   - Click "Fill"
   - All pixels should light to that color

3. **Chase**:
   - Animates a chase pattern
   - Good for verifying pixel order and count

**If LEDs Don't Light**:
- Verify hardware connections (see hardware_setup.md)
- Check pixel type matches (WS2811 vs WS2812B)
- Try different color order (RGB vs GRB)
- Check start channel and pixel count

### 2.4 Create Overlay Models

**Why**: Overlay models allow you to control zones independently from Home Assistant

**FPP Menu → Input/Output Setup → Pixel Overlay Models**

**Steps**:

1. **Enable Pixel Overlay**:
   - Overlay Enable: **Yes**

2. **Add Model #1**:
   - Name: `Front_Roofline` (no spaces, use underscores)
   - Type: `Channel Range`
   - Start Channel: `1`
   - Channel Count: `1500` (500 pixels × 3)
   - Orientation: `horizontal` (or `vertical` based on physical layout)
   - Direction: `L to R` (left to right, or as installed)
   - Color: `RGB` (or `GRB` if needed)

3. **Add Model #2**:
   - Name: `Side_Eaves`
   - Type: `Channel Range`
   - Start Channel: `1501`
   - Channel Count: `900` (300 pixels × 3)
   - Orientation: `horizontal`
   - Direction: `L to R`
   - Color: `RGB`

4. **Save Models**

**Test Overlay Models**:
- FPP Menu → Status/Control → Pixel Overlay Control
- Select model: `Front_Roofline`
- Set color (e.g., red)
- Click "Apply"
- Only Zone 1 should light up red

---

## Part 3: Home Assistant MQTT Integration

### 3.1 Install MQTT Broker (Mosquitto)

**If Home Assistant OS**:
1. **Settings → Add-ons → Add-on Store**
2. **Search for "Mosquitto broker"**
3. **Install**
4. **Start** and enable **Start on boot**
5. **Configuration** tab:
   ```yaml
   logins:
     - username: fpp
       password: your_secure_password
   anonymous: false
   ```
6. **Save** and **Restart** add-on

**If Home Assistant Container or Core**:
- Install Mosquitto separately on your system
- Configuration: `/etc/mosquitto/mosquitto.conf`
  ```
  listener 1883
  allow_anonymous false
  password_file /etc/mosquitto/passwd
  ```
- Create user: `mosquitto_passwd -c /etc/mosquitto/passwd fpp`

**Test MQTT Broker**:
- From another machine: `mosquitto_pub -h <ha-ip> -u fpp -P <password> -t test -m "hello"`
- Subscribe: `mosquitto_sub -h <ha-ip> -u fpp -P <password> -t test`

### 3.2 Configure MQTT in Home Assistant

**Settings → Devices & Services → Add Integration → MQTT**

1. **Broker**: `core-mosquitto` (if HA OS) or `<mqtt-server-ip>`
2. **Port**: `1883`
3. **Username**: `fpp`
4. **Password**: `<your_secure_password>`
5. **Submit**

**Verify MQTT Connected**:
- MQTT integration should show "Configured" status

### 3.3 Install FPP Home Assistant Plugin

**On FPP Web Interface**:

1. **FPP Menu → Content Setup → Plugins**
2. **Scroll to find "HomeAssistant"** (may need to expand categories)
3. **Click "Install"**
4. **Wait for installation** (30-60 seconds)
5. **Page will refresh**, plugin should show "Installed"

### 3.4 Configure FPP HA Plugin

**FPP Menu → Status/Control → HomeAssistant Plugin**

**Settings**:

1. **MQTT Settings**:
   - Broker Address: `<home-assistant-ip>` (e.g., `192.168.1.100`)
   - Port: `1883`
   - Username: `fpp`
   - Password: `<your_secure_password>`
   - Topic Prefix: `homeassistant` (default, leave as-is)

2. **Discovery Settings**:
   - Enable Discovery: **Yes**
   - Discovery Prefix: `homeassistant` (default)

3. **Model Selection**:
   - Select which overlay models to expose to HA:
     - ☑ Front_Roofline
     - ☑ Side_Eaves
   - (Check all models you created)

4. **Additional Options**:
   - Publish Status: **Yes** (allows HA to see FPP online/offline)
   - Publish Playlists: **Yes** (if you want to trigger playlists from HA)

5. **Save Configuration**

**Restart FPP Plugin**:
- May require FPP reboot: **Status/Control → System → Reboot**

### 3.5 Verify Discovery in Home Assistant

**Wait 30-60 seconds after FPP plugin configured**

**Settings → Devices & Services → MQTT**

Under "Devices", you should see:
- **FPP (<hostname>)** device
- Under this device, entities like:
  - `light.front_roofline` - Your Zone 1 overlay model
  - `light.side_eaves` - Your Zone 2 overlay model
  - `sensor.fpp_status` - FPP system status
  - `sensor.fpp_current_playlist` - Currently playing playlist

**If Not Appearing**:
1. Check FPP plugin configuration (MQTT broker IP, credentials)
2. Check MQTT broker logs in HA for connection attempts
3. Check FPP logs: **Status/Control → FPP Logs**
4. Restart both FPP and Home Assistant
5. Verify models are defined in FPP Pixel Overlay Models

### 3.6 Test Control from Home Assistant

**Developer Tools → Services**

1. **Turn On a Zone**:
   - Service: `light.turn_on`
   - Entity: `light.front_roofline`
   - Click "Call Service"
   - Zone 1 LEDs should light up (default color, likely white)

2. **Set Color**:
   - Service: `light.turn_on`
   - Entity: `light.front_roofline`
   - Service Data:
     ```yaml
     rgb_color: [255, 0, 0]
     ```
   - Click "Call Service"
   - Zone 1 should turn red

3. **Set Brightness**:
   - Service: `light.turn_on`
   - Entity: `light.front_roofline`
   - Service Data:
     ```yaml
     brightness: 128
     ```
   - Click "Call Service"
   - Zone 1 should dim to 50% brightness

4. **Turn Off**:
   - Service: `light.turn_off`
   - Entity: `light.front_roofline`
   - Click "Call Service"
   - Zone 1 should turn off

**Success**: You now have full HA control of your LEDs!

---

## Part 4: xLights Basics

### 4.1 Install xLights

**Download**: https://xlights.org/releases/

**Platforms**:
- Windows: `.exe` installer
- macOS: `.dmg` package
- Linux: `.AppImage` or package

**Installation**:
- Run installer, follow prompts
- Default installation path is fine
- Launch xLights after install

### 4.2 Create Show Directory

**First Launch**:
- xLights will ask for a "Show Directory"
- Create new folder: `C:\Users\<you>\Documents\xLights\HomeChristmas` (Windows)
- Or: `~/Documents/xLights/HomeChristmas` (Mac/Linux)
- This folder will store all your sequences, media, and configurations

### 4.3 Setup Wizard

**xLights will guide you through initial setup**:

1. **Show Folder**: Already set
2. **Controller Setup**:
   - Add Controller: "Falcon Player (FPP)"
   - IP Address: `<fpp-ip-address>` (e.g., `192.168.1.50`)
   - Description: "Main FPP Controller"
   - Click "Discover" to auto-populate outputs
3. **Auto-layout models**: Skip for now (manual setup better)
4. **Complete Setup**

### 4.4 Create Layout (House Model)

**Purpose**: Visual representation of your house for sequencing

**Steps**:

1. **Layout Tab** (bottom of xLights window)
2. **Add → Model → Single Line**:
   - Name: `Front_Roofline`
   - Display As: `Single Line`
   - String Type: `RGB Nodes`
   - Controller: Select your FPP controller
   - Start Channel: `1`
   - Number of Nodes: `500`
   - Nodes per String: `500`
   - Strings: `1`
   - Click "OK"

3. **Position the Model**:
   - Drag the model in the Layout preview to match your house
   - Resize as needed
   - Rotate if necessary (right-click → Rotate)

4. **Add Second Model** (Zone 2):
   - Add → Model → Single Line
   - Name: `Side_Eaves`
   - Start Channel: `1501`
   - Number of Nodes: `300`
   - Position on layout

5. **Add House Background** (optional):
   - File → Preferences → Layouts → Background Image
   - Upload photo of your house
   - Overlay models on the photo for accurate representation

6. **Save Layout**: File → Save

### 4.5 Create a Simple Sequence

**What**: A sequence is a timed animation of lighting effects

**Tutorial Sequence**:

1. **Sequencer Tab** (top of xLights)
2. **New Sequence**:
   - Type: **Musical Sequence** (or **Animation** for no music)
   - Audio File: Browse to MP3/WAV file (optional, can add later)
   - Duration: 30 seconds (for testing)
   - Click "OK"

3. **Timeline Layout**:
   - Left side: List of your models (Front_Roofline, Side_Eaves)
   - Top: Timeline (in seconds)
   - Bottom: Waveform (if music file selected)

4. **Add Effects**:
   - Right side: Effects panel (Color Wash, Chase, Twinkle, etc.)
   - Drag "Color Wash" effect onto `Front_Roofline` at 0 seconds
   - Duration: 10 seconds (drag end marker)
   - Click effect to modify:
     - Set color to red
   - Drag "Chase" effect onto `Front_Roofline` at 10 seconds
     - Duration: 10 seconds
     - Color: blue
   - Drag "On" effect onto `Side_Eaves` at 20 seconds
     - Duration: 10 seconds
     - Color: green

5. **Preview in xLights**:
   - Click "Play" button (bottom left)
   - House layout should animate with effects

6. **Save Sequence**: File → Save

### 4.6 Upload Sequence to FPP

**Two Methods**:

**Method 1: Direct Upload from xLights**
1. **Tools → FPP Connect**
2. **Select FPP Controller** from list
3. **Upload Sequence**:
   - Select your `.fseq` sequence file (auto-saved in show directory)
   - Click "Upload"
4. **FPP will show sequence in playlist**

**Method 2: Manual Upload via FPP Web UI**
1. **FPP → Content Setup → File Manager**
2. **Navigate to**: `sequences/`
3. **Upload Sequences**: Click "Upload" button
4. **Browse** to xLights show directory → `sequences/` → select `.fseq` file
5. **Upload** and wait for completion

### 4.7 Play Sequence from FPP

**FPP → Content Setup → Playlists**

1. **Create New Playlist**:
   - Name: `Test Playlist`
   - Click "Add"

2. **Add Sequence to Playlist**:
   - Drag sequence from "Available Sequences" to playlist
   - Or click "+" button

3. **Save Playlist**

**Play Playlist**:
- **FPP → Status/Control → Status Page**
- **Select Playlist**: `Test Playlist`
- **Click "Play"**
- Your LEDs should animate according to sequence!

**Success**: You've created and played your first xLights sequence!

---

## Part 5: Advanced FPP Features

### 5.1 Scheduling

**FPP → Content Setup → Scheduler**

**Create Schedule Entry**:
1. **Enable**: Yes
2. **Playlist**: `Test Playlist`
3. **Day Restrictions**: Select days (e.g., Friday, Saturday, Sunday)
4. **Start Time**: `18:00` (6 PM)
5. **End Time**: `22:00` (10 PM)
6. **Repeat**: Yes (loops playlist during window)
7. **Save**

**Result**: FPP automatically plays your sequence every Friday-Sunday, 6-10 PM

### 5.2 FPP Remote Management

**FPP Mobile App** (iOS/Android):
- Search "Falcon Player Remote" or "FPP Remote"
- Connect to FPP IP address
- Control playlists, test outputs, view status from phone

**Web Bookmarks**:
- Bookmark FPP web interface on devices for quick access
- Create shortcuts for Display Testing, Playlist control

### 5.3 FPP Logging & Diagnostics

**Check FPP Logs**:
- **Status/Control → FPP Logs**
- Tail logs in real-time to debug issues
- Error messages will appear here

**System Information**:
- **Status/Control → System Info**
- CPU/memory usage, uptime, temperatures
- Useful for performance monitoring

---

## Part 6: Home Assistant Automation Examples

### 6.1 Simple On/Off Automation

**Automation: Turn on lights at sunset**

```yaml
automation:
  - alias: "Christmas Lights On at Sunset"
    trigger:
      - platform: sun
        event: sunset
        offset: "-00:30:00"  # 30 minutes before sunset
    action:
      - service: light.turn_on
        target:
          entity_id:
            - light.front_roofline
            - light.side_eaves
        data:
          rgb_color: [255, 255, 255]  # White
          brightness: 200  # 78% brightness
```

**Automation: Turn off lights at 11 PM**

```yaml
automation:
  - alias: "Christmas Lights Off at 11 PM"
    trigger:
      - platform: time
        at: "23:00:00"
    action:
      - service: light.turn_off
        target:
          entity_id:
            - light.front_roofline
            - light.side_eaves
```

### 6.2 Color Scene Automation

**Scene: Holiday Colors**

```yaml
scene:
  - name: "Christmas Red Green"
    entities:
      light.front_roofline:
        state: on
        rgb_color: [255, 0, 0]  # Red
        brightness: 255
      light.side_eaves:
        state: on
        rgb_color: [0, 255, 0]  # Green
        brightness: 255
```

**Automation: Apply scene on demand**

```yaml
automation:
  - alias: "Christmas Mode Button"
    trigger:
      - platform: state
        entity_id: input_boolean.christmas_mode
        to: "on"
    action:
      - service: scene.turn_on
        target:
          entity_id: scene.christmas_red_green
```

### 6.3 Voice Control

**If using Google Assistant or Alexa via HA**:

- Expose light entities to voice assistant (HA integrations)
- Say: "Hey Google, turn on the Christmas lights"
- Say: "Alexa, set Christmas lights to blue"

---

## Useful Resources & Tutorials

### FPP Documentation
- **Official Manual**: https://falconchristmas.github.io/FPP/
- **FPP YouTube Channel**: https://www.youtube.com/c/FalconChristmas
- **FPP Forums**: https://falconchristmas.com/forum/

### xLights Tutorials
- **xLights YouTube (official)**: https://www.youtube.com/@xLights
- **Canispater xLights Tutorials**: Beginner-friendly series on YouTube
- **xLights Manual (Wiki)**: https://xlights.org/docs/

### Home Assistant Integration
- **HA MQTT Integration**: https://www.home-assistant.io/integrations/mqtt/
- **HA Light Entity Docs**: https://www.home-assistant.io/integrations/light/
- **HA Community Forum**: https://community.home-assistant.io/

### Community Support
- **AusChristmasLighting**: https://auschristmaslighting.com/ (Australia-focused)
- **PlanetChristmas Forums**: https://www.planetchristmas.com/forums/ (US-focused)
- **r/ChristmasLights**: https://www.reddit.com/r/ChristmasLights/ (Reddit community)

---

## Next Steps

1. **Complete FPP configuration** as outlined above
2. **Test each zone independently** with overlay control
3. **Set up Home Assistant integration** for daily control
4. **Learn xLights basics** with simple sequences
5. **Create holiday-specific sequences** (Christmas, Halloween, etc.)
6. **Schedule automated shows** for special occasions

**Refer to**:
- [home_assistant_integration.md](./home_assistant_integration.md) for detailed HA configuration examples
- [hardware_setup.md](./hardware_setup.md) if you encounter output issues

---

**Updated**: 2025-12-24
