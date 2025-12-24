# Home Assistant Integration Guide

## Overview

This guide provides detailed instructions for integrating the Falcon Player (FPP) LED controller with Home Assistant, including MQTT configuration, entity customization, automation examples, and updates to the `richardthorek/haconfiguration` repository.

---

## Prerequisites

- ✅ FPP installed and configured on Raspberry Pi
- ✅ LEDs tested and working via FPP
- ✅ Home Assistant instance running and accessible
- ✅ MQTT broker (Mosquitto) installed on Home Assistant
- ✅ FPP Home Assistant plugin installed and configured

---

## Part 1: MQTT Broker Configuration (Detailed)

### 1.1 Mosquitto Add-on Installation (Home Assistant OS)

**If using Home Assistant OS**:

1. **Navigate**: Settings → Add-ons → Add-on Store
2. **Search**: "Mosquitto broker"
3. **Install**: Click "Install" and wait for completion
4. **Configuration**:
   - Click "Configuration" tab
   - Edit YAML:
     ```yaml
     logins:
       - username: fpp
         password: fpp_secure_password_here
       - username: homeassistant
         password: ha_secure_password_here
     anonymous: false
     customize:
       active: false
       folder: mosquitto
     certfile: fullchain.pem
     keyfile: privkey.pem
     require_certificate: false
     ```
5. **Save Configuration**
6. **Start**: Info tab → "Start"
7. **Enable**: "Start on boot"

### 1.2 Mosquitto Manual Installation (Docker/Core)

**For Docker**:

```bash
docker run -d \
  --name mosquitto \
  --restart unless-stopped \
  -p 1883:1883 \
  -p 9001:9001 \
  -v /path/to/mosquitto/config:/mosquitto/config \
  -v /path/to/mosquitto/data:/mosquitto/data \
  -v /path/to/mosquitto/log:/mosquitto/log \
  eclipse-mosquitto:latest
```

**Configuration file** (`/path/to/mosquitto/config/mosquitto.conf`):

```
listener 1883
allow_anonymous false
password_file /mosquitto/config/passwd

persistence true
persistence_location /mosquitto/data/

log_dest file /mosquitto/log/mosquitto.log
log_type all
```

**Create users**:

```bash
docker exec -it mosquitto mosquitto_passwd -c /mosquitto/config/passwd fpp
docker exec -it mosquitto mosquitto_passwd /mosquitto/config/passwd homeassistant
docker restart mosquitto
```

**For Home Assistant Core (manual install)**:

```bash
sudo apt-get install mosquitto mosquitto-clients
sudo mosquitto_passwd -c /etc/mosquitto/passwd fpp
sudo mosquitto_passwd /etc/mosquitto/passwd homeassistant
sudo systemctl enable mosquitto
sudo systemctl start mosquitto
```

---

## Part 2: Home Assistant MQTT Integration Setup

### 2.1 Add MQTT Integration

**Settings → Devices & Services → Add Integration**

1. Search: "MQTT"
2. **Broker**:
   - If HA OS with Mosquitto add-on: `core-mosquitto`
   - If external broker: IP address (e.g., `192.168.1.100`)
3. **Port**: `1883` (default)
4. **Username**: `homeassistant`
5. **Password**: Your HA MQTT password
6. **Submit**

**Verify**:
- Integration should show "Configured" with green check
- Devices: Should show 0 initially, will populate after FPP discovery

### 2.2 Test MQTT Connection

**Developer Tools → Services**

**Publish Test Message**:

```yaml
service: mqtt.publish
data:
  topic: "test/message"
  payload: "Hello from Home Assistant"
```

**Subscribe** (Developer Tools → MQTT):

- Topic: `test/#`
- Click "Start Listening"
- Publish the message above
- You should see it appear in the subscription log

---

## Part 3: FPP Home Assistant Plugin Configuration (Detailed)

### 3.1 Plugin Installation Verification

**FPP → Content Setup → Plugins**

- Find "HomeAssistant" in list
- Status should be "Installed"
- If not installed: Click "Install", wait, and refresh

### 3.2 Plugin Configuration

**FPP → Status/Control → Plugins → HomeAssistant**

**MQTT Broker Settings**:

```
Broker Address: 192.168.1.100  (Your HA IP)
Port: 1883
Username: fpp
Password: fpp_secure_password_here
Client ID: fpp_<hostname>  (auto-generated, leave as-is)
Topic Prefix: homeassistant
```

**Discovery Settings**:

```
Enable Discovery: ☑ Yes
Discovery Prefix: homeassistant
Device Name: FPP LED Controller  (or custom name)
```

**Models to Expose**:

- ☑ Front_Roofline
- ☑ Side_Eaves
- ☑ (Any other overlay models you've created)

**Advanced Options**:

```
Publish Status: ☑ Yes
Publish Playlists: ☑ Yes
Publish Sequences: ☐ No (optional)
Status Update Interval: 30 (seconds)
```

**Save** and **Restart FPP** (System → Reboot)

### 3.3 Verify MQTT Communication

**On FPP**:
- **Status/Control → FPP Logs**
- Look for lines like:
  ```
  HomeAssistant: Connected to MQTT broker
  HomeAssistant: Publishing discovery for model: Front_Roofline
  ```

**On Home Assistant**:
- **Settings → Devices & Services → MQTT → Devices**
- You should see:
  - **FPP LED Controller** device
  - With entities:
    - `light.front_roofline`
    - `light.side_eaves`
    - `sensor.fpp_status`
    - `sensor.fpp_current_playlist`
    - (Others depending on configuration)

---

## Part 4: Entity Customization in Home Assistant

### 4.1 Customize Light Names

**Settings → Devices & Services → MQTT → Devices → FPP LED Controller**

**For each light entity**:

1. Click entity (e.g., `light.front_roofline`)
2. Click gear icon (top right)
3. **Name**: "Front Roofline" (display name)
4. **Entity ID**: `light.front_roofline` (or customize)
5. **Icon**: `mdi:string-lights` or `mdi:led-strip-variant`
6. **Area**: Assign to area (e.g., "Exterior")
7. **Labels**: Add labels (e.g., "Christmas", "Outdoor")
8. **Update**

### 4.2 Create Light Groups

**For controlling multiple zones together**:

**Settings → Devices & Services → Helpers → Create Helper → Group → Light Group**

**Example: "All Christmas Lights"**

- **Name**: "All Christmas Lights"
- **Entities**:
  - ☑ light.front_roofline
  - ☑ light.side_eaves
- **Create**

**Result**: `light.all_christmas_lights` entity that controls all zones

### 4.3 Customize Lovelace Dashboard

**Example Dashboard Card**:

```yaml
type: light
entity: light.all_christmas_lights
name: Christmas Lights
icon: mdi:string-lights
```

**Advanced Card with Individual Zones**:

```yaml
type: entities
title: Christmas Light Zones
entities:
  - entity: light.all_christmas_lights
    name: All Zones
  - type: divider
  - entity: light.front_roofline
    name: Front Roofline
  - entity: light.side_eaves
    name: Side Eaves
```

**Color Picker Card**:

```yaml
type: light
entity: light.front_roofline
name: Front Roofline
```

(Built-in card includes color picker automatically)

---

## Part 5: Automation Examples

### 5.1 Basic On/Off Automations

**Automation 1: Lights On at Sunset**

```yaml
automation:
  - id: christmas_lights_sunset_on
    alias: "Christmas Lights - On at Sunset"
    description: "Turn on Christmas lights 30 minutes before sunset"
    trigger:
      - platform: sun
        event: sunset
        offset: "-00:30:00"
    condition: []
    action:
      - service: light.turn_on
        target:
          entity_id: light.all_christmas_lights
        data:
          brightness: 255
          rgb_color: [255, 200, 100]  # Warm white
    mode: single
```

**Automation 2: Lights Off at Midnight**

```yaml
automation:
  - id: christmas_lights_midnight_off
    alias: "Christmas Lights - Off at Midnight"
    description: "Turn off Christmas lights at midnight"
    trigger:
      - platform: time
        at: "00:00:00"
    condition: []
    action:
      - service: light.turn_off
        target:
          entity_id: light.all_christmas_lights
    mode: single
```

### 5.2 Holiday-Specific Automations

**Automation: Christmas Colors on December**

```yaml
automation:
  - id: christmas_colors_december
    alias: "Christmas Lights - December Colors"
    description: "Apply red/green colors during December when lights turn on"
    trigger:
      - platform: state
        entity_id: light.all_christmas_lights
        to: "on"
    condition:
      - condition: template
        value_template: "{{ now().month == 12 }}"
    action:
      - service: light.turn_on
        target:
          entity_id: light.front_roofline
        data:
          rgb_color: [255, 0, 0]  # Red
          brightness: 255
      - service: light.turn_on
        target:
          entity_id: light.side_eaves
        data:
          rgb_color: [0, 255, 0]  # Green
          brightness: 255
    mode: single
```

### 5.3 Weather-Reactive Automations

**Automation: Dim Lights in Rain**

```yaml
automation:
  - id: christmas_lights_rain_dim
    alias: "Christmas Lights - Dim in Rain"
    description: "Reduce brightness when it's raining"
    trigger:
      - platform: state
        entity_id: weather.home
        attribute: condition
        to: "rainy"
    condition:
      - condition: state
        entity_id: light.all_christmas_lights
        state: "on"
    action:
      - service: light.turn_on
        target:
          entity_id: light.all_christmas_lights
        data:
          brightness: 128  # 50% brightness
    mode: single
```

### 5.4 Motion-Triggered Effects

**Automation: Bright White on Front Door Motion**

```yaml
automation:
  - id: christmas_lights_motion_bright
    alias: "Christmas Lights - Motion Bright"
    description: "Turn front lights bright white on motion"
    trigger:
      - platform: state
        entity_id: binary_sensor.front_door_motion
        to: "on"
    condition:
      - condition: state
        entity_id: light.front_roofline
        state: "on"
    action:
      - service: light.turn_on
        target:
          entity_id: light.front_roofline
        data:
          rgb_color: [255, 255, 255]
          brightness: 255
      - delay:
          minutes: 5
      - service: light.turn_on
        target:
          entity_id: light.front_roofline
        data:
          rgb_color: [255, 200, 100]  # Back to warm white
          brightness: 200
    mode: restart
```

### 5.5 FPP Playlist Trigger

**Automation: Play Christmas Playlist on Special Button**

```yaml
automation:
  - id: play_christmas_show
    alias: "Play Christmas Light Show"
    description: "Trigger FPP playlist from Home Assistant button"
    trigger:
      - platform: state
        entity_id: input_boolean.christmas_show_mode
        to: "on"
    condition: []
    action:
      - service: mqtt.publish
        data:
          topic: "fpp/set/playlist/start"
          payload: "Christmas_Main_Show"
    mode: single
```

**Helper Required**:
- Create Input Boolean: Settings → Devices & Services → Helpers → Toggle
- Name: "Christmas Show Mode"
- Entity ID: `input_boolean.christmas_show_mode`

---

## Part 6: Advanced Integration Features

### 6.1 Status Monitoring

**Create Status Card**:

```yaml
type: entities
title: FPP Controller Status
entities:
  - entity: sensor.fpp_status
    name: Controller Status
  - entity: sensor.fpp_current_playlist
    name: Current Playlist
  - entity: sensor.fpp_uptime
    name: Uptime
  - entity: sensor.fpp_temperature
    name: Temperature
```

### 6.2 Notifications on Controller Issues

**Automation: Alert if FPP Goes Offline**

```yaml
automation:
  - id: fpp_offline_alert
    alias: "FPP Controller - Offline Alert"
    description: "Send notification if FPP goes offline"
    trigger:
      - platform: state
        entity_id: sensor.fpp_status
        to: "unavailable"
        for:
          minutes: 5
    condition: []
    action:
      - service: notify.mobile_app
        data:
          title: "FPP Controller Offline"
          message: "The Christmas light controller has gone offline. Please check the system."
          data:
            priority: high
    mode: single
```

### 6.3 Voice Control Setup

**Google Assistant**:

1. Settings → Devices & Services → Google Assistant → Configure
2. Expose entities:
   - ☑ light.all_christmas_lights
   - ☑ light.front_roofline
   - ☑ light.side_eaves
3. Sync devices in Google Home app
4. Voice commands:
   - "Hey Google, turn on Christmas lights"
   - "Hey Google, set Christmas lights to red"
   - "Hey Google, dim the Christmas lights"

**Amazon Alexa**:

1. Settings → Devices & Services → Alexa → Configure
2. Expose entities (same as above)
3. Discover devices in Alexa app
4. Voice commands:
   - "Alexa, turn on Christmas lights"
   - "Alexa, set Christmas lights to blue"

---

## Part 7: Updates to `richardthorek/haconfiguration` Repository

### 7.1 Recommended Repository Structure

**Add these files to your HA configuration repo**:

```
haconfiguration/
├── automations/
│   └── christmas_lights.yaml          # All Christmas light automations
├── scenes/
│   └── christmas_lights.yaml          # Pre-defined color scenes
├── scripts/
│   └── christmas_lights.yaml          # Reusable light scripts
├── packages/
│   └── christmas_lights.yaml          # Package for all related config
├── lovelace/
│   └── christmas_lights_dashboard.yaml # Dashboard configuration
└── README.md                           # Updated with Christmas lights section
```

### 7.2 Package Configuration (Recommended Approach)

**File: `packages/christmas_lights.yaml`**

```yaml
# Christmas Lights Package
# Combines all Christmas light-related configuration

# Input Booleans
input_boolean:
  christmas_show_mode:
    name: Christmas Show Mode
    icon: mdi:playlist-play
  
  christmas_schedule_enabled:
    name: Christmas Schedule Enabled
    icon: mdi:calendar-clock
    initial: true

# Input Select (Color Presets)
input_select:
  christmas_light_preset:
    name: Christmas Light Preset
    options:
      - "Warm White"
      - "Cool White"
      - "Red & Green"
      - "Blue & White"
      - "Rainbow"
      - "Orange (Halloween)"
    icon: mdi:palette

# Scenes
scene:
  - name: "Christmas Lights - Warm White"
    entities:
      light.all_christmas_lights:
        state: on
        brightness: 255
        rgb_color: [255, 200, 100]
  
  - name: "Christmas Lights - Red Green"
    entities:
      light.front_roofline:
        state: on
        brightness: 255
        rgb_color: [255, 0, 0]
      light.side_eaves:
        state: on
        brightness: 255
        rgb_color: [0, 255, 0]

# Automations
automation:
  - id: christmas_lights_sunset_on
    alias: "Christmas Lights - On at Sunset"
    trigger:
      - platform: sun
        event: sunset
        offset: "-00:30:00"
    condition:
      - condition: state
        entity_id: input_boolean.christmas_schedule_enabled
        state: "on"
    action:
      - service: light.turn_on
        target:
          entity_id: light.all_christmas_lights
        data:
          brightness: 255
  
  - id: christmas_lights_midnight_off
    alias: "Christmas Lights - Off at Midnight"
    trigger:
      - platform: time
        at: "00:00:00"
    condition:
      - condition: state
        entity_id: input_boolean.christmas_schedule_enabled
        state: "on"
    action:
      - service: light.turn_off
        target:
          entity_id: light.all_christmas_lights

# Scripts
script:
  christmas_lights_preset_apply:
    alias: "Apply Christmas Light Preset"
    sequence:
      - choose:
          - conditions:
              - condition: state
                entity_id: input_select.christmas_light_preset
                state: "Warm White"
            sequence:
              - service: scene.turn_on
                target:
                  entity_id: scene.christmas_lights_warm_white
          
          - conditions:
              - condition: state
                entity_id: input_select.christmas_light_preset
                state: "Red & Green"
            sequence:
              - service: scene.turn_on
                target:
                  entity_id: scene.christmas_lights_red_green
```

### 7.3 Enable Packages in Configuration

**File: `configuration.yaml`**

Add (if not already present):

```yaml
homeassistant:
  packages: !include_dir_named packages
```

**Restart Home Assistant** after adding packages

### 7.4 Update README.md

**Add section to `richardthorek/haconfiguration/README.md`**:

```markdown
## Christmas Lights Integration

This Home Assistant configuration includes integration with the Falcon Player (FPP) LED controller for home Christmas lighting.

### Components

- **Controller**: Raspberry Pi 4 + Falcon PiCap running FPP
- **LEDs**: 12V WS2811 addressable pixels
- **Integration**: MQTT via FPP Home Assistant plugin
- **Entities**: 
  - `light.front_roofline` - Front roofline zone
  - `light.side_eaves` - Side eaves zone
  - `light.all_christmas_lights` - Group for all zones

### Configuration Files

- `packages/christmas_lights.yaml` - Main configuration package
- `automations/christmas_lights.yaml` - Supplemental automations
- `lovelace/christmas_lights_dashboard.yaml` - Dashboard config

### Automations

- **Sunset On**: Lights turn on 30 minutes before sunset
- **Midnight Off**: Lights turn off at midnight
- **Motion Bright**: Front lights brighten on motion detection
- **Playlist Triggers**: Start FPP playlists via HA

### Voice Control

Exposed to Google Assistant and Alexa:
- "Turn on Christmas lights"
- "Set Christmas lights to [color]"
- "Dim Christmas lights"

### Related Documentation

See the [home-lighting repository](https://github.com/richardthorek/home-lighting) for hardware setup, FPP configuration, and xLights sequencing guides.
```

### 7.5 Commit Changes

```bash
cd /path/to/haconfiguration
git add packages/christmas_lights.yaml
git add README.md
git commit -m "Add Christmas Lights FPP integration package"
git push origin main
```

---

## Part 8: Testing & Validation

### 8.1 Test Checklist

- ☐ MQTT broker connected and stable
- ☐ FPP HA plugin publishing to MQTT (check logs)
- ☐ Light entities appear in HA
- ☐ Manual light control works (color, brightness, on/off)
- ☐ Group control works (all_christmas_lights)
- ☐ Automations trigger as expected
- ☐ Voice control responds (if configured)
- ☐ Status sensors update (FPP status, playlist, etc.)
- ☐ Lovelace dashboard displays correctly

### 8.2 Troubleshooting

**Lights Don't Appear in HA**:
- Check FPP plugin config (MQTT broker IP, credentials)
- Verify MQTT broker running (HA → Mosquitto add-on)
- Check FPP logs for MQTT connection errors
- Restart both FPP and HA

**Colors Don't Match**:
- Check color order in FPP (RGB vs GRB)
- Verify overlay model settings
- Test directly in FPP Display Testing first

**Automations Don't Trigger**:
- Check automation conditions (time, state, etc.)
- Enable automation trace (Settings → Automations → Select automation → Traces)
- Verify entities are correct in automation YAML

**Status Sensors Unavailable**:
- Verify "Publish Status" enabled in FPP HA plugin
- Check MQTT topic subscription: `homeassistant/sensor/fpp_#`
- Restart FPP plugin

---

## Next Steps

1. **Customize Lovelace dashboard** with your preferences
2. **Create more scenes** for different occasions
3. **Expand automations** based on your routines
4. **Learn xLights** for custom sequences triggered from HA
5. **Share your configuration** with the community (if desired)

**Related Guides**:
- [software_setup.md](./software_setup.md) - FPP and xLights configuration
- [hardware_setup.md](./hardware_setup.md) - Hardware troubleshooting
- [master_plan.md](../../master_plan.md) - Project overview

---

**Updated**: 2025-12-24
