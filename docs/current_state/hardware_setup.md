# Hardware Setup Guide - Home LED Lighting Project

## Overview

This guide covers physical assembly, wiring, and testing of the Falcon PiCap + Raspberry Pi LED controller system. Follow these steps carefully to ensure a safe, reliable installation.

**Current Build Configuration** (as of 2025-12-24):
- 500× 12V WS2811 bullet pixels, 10cm spacing
- Mean Well LRS-350-12 (350W) power supply
- 2 zones, both-ends power injection strategy
- Wired Ethernet connection

**Prerequisites**:
- All parts received from shopping list
- Basic tools available (see Tools section)
- Work area with good lighting
- Multimeter for testing (highly recommended)

---

## Phase 1: Component Verification (30 minutes)

### 1.1 Inventory Check

Unbox and verify all components against your shopping list:

**Controller Components**:
- ☐ Raspberry Pi 4 (4GB recommended)
- ☐ Falcon PiCap v2 HAT
- ☐ MicroSD card (32GB, Class 10)
- ☐ Power supply (12V, appropriate wattage)
- ☐ Ethernet cable
- ☐ Optional: Pi case compatible with HAT

**LED Components**:
- ☐ LED pixels/strips (count matches order)
- ☐ Ray Wu 3-pin connectors (10-20 pairs)
- ☐ Test string (50-100 pixels) separate if ordered

**Wiring**:
- ☐ 18AWG 3-conductor cable (30m+)
- ☐ 16AWG 2-conductor cable (15m+)
- ☐ Heat shrink tubing assortment
- ☐ Wire connectors/terminal blocks

**Safety & Mounting**:
- ☐ Fuses and fuse holders
- ☐ Enclosure(s) for controller and PSU
- ☐ Mounting hardware (clips, strips, channels)
- ☐ Silicone sealant, dielectric grease

### 1.2 Visual Inspection

Check each item for shipping damage:
- Pi and PiCap: No bent pins, cracked boards
- LEDs: Inspect 5-10 random pixels for damage
- Connectors: Check waterproof seals intact
- PSU: No dents, rattling (loose internals)

**Document with photos** for potential warranty claims

---

## Phase 2: Test Bench Assembly (2-3 hours)

### 2.1 Prepare the Raspberry Pi

**Important**: Complete software installation (see [software_setup.md](./software_setup.md)) before attaching PiCap

1. **Flash FPP to MicroSD card** (detailed in software_setup.md)
2. **Insert SD card** into Raspberry Pi
3. **Initial Pi boot** (without PiCap attached):
   - Connect Pi to HDMI monitor (or SSH)
   - Connect Ethernet
   - Power via USB-C
   - Verify FPP boots and accessible at `http://fpp.local`
4. **Shutdown Pi** before mounting PiCap

### 2.2 Install Falcon PiCap HAT

**Safety**: Work on anti-static mat or ground yourself before handling

**Steps**:
1. **Orient the PiCap** correctly:
   - 40-pin GPIO header aligns with Pi GPIO
   - PiCap sits on top of Pi
   - Output terminals face away from Pi USB ports

2. **Mount PiCap on Pi**:
   - Align 40-pin header carefully (DO NOT FORCE)
   - Press down gently until fully seated
   - All pins should be invisible (fully inserted)

3. **Secure with standoffs** (if included):
   - Screw standoffs into Pi mounting holes
   - This prevents board flex and stress on GPIO

4. **Inspect connection**:
   - No bent pins visible
   - Board sits level on Pi
   - No gaps between boards

**Photo Documentation**: Take photo of assembled Pi + PiCap

### 2.3 Power Supply Setup

**Warning**: AC mains power is dangerous. If you're not comfortable with AC wiring, hire an electrician.

**For Test Bench** (indoor, temporary):

1. **PSU Location**: Place on non-conductive surface (wood bench, plastic sheet)

2. **AC Input Wiring** (if hardwiring):
   - **TURN OFF BREAKER** before connecting AC
   - Connect Live (brown/black) to PSU L terminal
   - Connect Neutral (blue/white) to PSU N terminal  
   - Connect Earth/Ground (green/yellow) to PSU ground terminal
   - Use proper AC-rated wire (14AWG minimum for 15A circuit)
   - **Double-check polarity** before powering on

3. **AC Input** (if using plug):
   - Most enclosed PSUs have IEC C14 inlet (standard PC power cord)
   - Use appropriate voltage cord (120V US or 240V AU/EU)

4. **DC Output Terminals**:
   - **V+** terminal (red label or +12V)
   - **V-** terminal (black label or COM/GND)

5. **Initial Power Test** (no load):
   - Turn on PSU (some have switch)
   - Check LED indicator (if present) lights green
   - Use multimeter to verify: **11.5-12.5V DC** between V+ and V-
   - If voltage too high (>13V) or too low (<11V), do not use PSU

**Safety Checks**:
- ☐ No exposed AC terminals
- ☐ PSU not overheating (should be cool/warm to touch)
- ☐ No buzzing or unusual sounds

### 2.4 Wire the Test LED String

**Materials Needed**:
- 50-100 pixel test string with pigtail connectors
- 18AWG 3-conductor wire (2 meters)
- Ray Wu 3-pin connectors (male/female pair)
- Heat shrink tubing
- Wire strippers, crimping tool

**PiCap Output Connector Pinout** (verify with PiCap manual, but typically):
```
Output Terminal Block (3-position):
[V+] [GND] [DATA]
```

**WS2811 Pixel Input Connector Pinout**:
```
Ray Wu 3-pin (female on first pixel):
Red = V+ (12V)
Black/Blue = GND (ground)
White/Green = Data In
```

**Wiring Steps**:

1. **Create PiCap-to-Pixel Cable**:
   
   **Option A: Direct screw terminal to pigtail**
   - Strip 3 feet of 18AWG 3-conductor cable
   - One end: Strip and tin wires, insert into PiCap Output 1 terminals:
     - Red → V+ terminal
     - Black → GND terminal
     - White/Green → DATA terminal
   - Other end: Attach male Ray Wu 3-pin connector:
     - Red → Red (V+)
     - Black → Black/Blue (GND)
     - White/Green → White/Green (Data)
   - Crimp connector pins, secure in housing, heat shrink each wire

   **Option B: Using screw terminal blocks**
   - Create cable from PiCap to a screw terminal block
   - Create cable from terminal block to Ray Wu connector
   - Allows easier reconfiguration

2. **Connect Test Pixel String**:
   - Plug male connector (from PiCap cable) into female connector (first pixel)
   - Ensure connection is snug and twisted on (Ray Wu connectors screw on)

3. **Power Connection for PiCap**:
   - PiCap can be powered from pixel supply (recommended for test)
   - Connect PSU output to PiCap power input terminals:
     - PSU V+ → PiCap V+ input
     - PSU V- → PiCap GND input
   - This powers both PiCap and Raspberry Pi (PiCap has regulator)

**Complete Wiring Diagram** (test bench):
```
[12V PSU]
   |
   +--- V+ ----┬---→ PiCap V+ Input ----→ Output 1 V+ ----→ Pixels V+
   |           │
   +--- GND ---┴---→ PiCap GND Input ----→ Output 1 GND ---→ Pixels GND
                                           Output 1 DATA ---→ Pixels Data In

[Pi Ethernet] ---→ Network Router

(PiCap regulates 12V down to 5V for Pi via GPIO power)
```

**Double-Check Before Power On**:
- ☐ All polarities correct (V+ to V+, GND to GND)
- ☐ No short circuits (use multimeter continuity test)
- ☐ Data line connected correctly (not swapped with power)
- ☐ Pixel string data direction correct (arrow on pixels flows away from controller)
- ☐ Power supply voltage verified at 12V
- ☐ Ethernet cable connected

### 2.5 First Power-On Test

**Procedure**:

1. **Power on 12V PSU**
   - Pi should boot (ACT LED flashing on Pi)
   - PiCap LED indicators should light (if present)

2. **Wait for FPP boot** (30-60 seconds)
   - Green activity LED on Pi should show SD card access

3. **Access FPP Web UI**:
   - From computer on same network: `http://fpp.local` or `http://<pi-ip-address>`
   - FPP dashboard should load

4. **Navigate to FPP Display Testing**:
   - FPP Menu → Status/Control → Display Testing
   - Set "Output" to "Output 1"
   - Set "Start Channel" to 1
   - Set "End Channel" to (pixels × 3), e.g., 50 pixels = 150 channels
   - Enable "Test Mode"
   - Click "Cycle Colors"

**Expected Result**: Test pixels should light up in sequence (red, green, blue cycling)

**Troubleshooting** (if no light):

| Symptom | Likely Cause | Solution |
|---------|--------------|----------|
| No LEDs light at all | No power to pixels, or wrong voltage | Check PSU voltage, check V+ and GND connections |
| LEDs flicker random colors | Data line not connected or wrong pixel type | Verify data wire, check FPP pixel type setting |
| Only first LED lights | Break in string or wrong start channel | Test pixel string with external tool, verify channel config |
| Pi won't boot | SD card issue or insufficient power | Re-flash SD card, check 12V PSU amperage |
| FPP web UI not accessible | Network issue or FPP not booted | Check Ethernet connection, ping Pi IP address |

**Success Criteria**:
- ☐ 50-100 test pixels all light up
- ☐ Colors accurate (red looks red, blue looks blue, not swapped)
- ☐ No flickering or dimming
- ☐ FPP responds to color/effect changes immediately

**Document**: Take video of test pixels cycling colors

---

## Phase 3: Configure FPP Outputs (30 minutes)

See [software_setup.md](./software_setup.md) for detailed FPP configuration, but key points:

### 3.1 Channel Outputs Setup

1. **FPP Menu → Input/Output Setup → Channel Outputs**
2. **Enable Output #1**:
   - Type: `PiCap`
   - Description: `Test String`
   - Start Channel: `1`
   - Pixel Count: `50` (or your test string count)
   - Color Order: `RGB` (or verify with test - if colors wrong try `GRB`)
   - Chip Type: `WS2811` (for 12V pixels) or `WS2812B` (for 5V)
3. **Save Settings**
4. **Test again with Display Testing** to verify

### 3.2 Verify Pixel Count

- Use FPP Display Testing to light up pixel-by-pixel
- Count physical LEDs to ensure FPP pixel count matches actual string

### 3.3 Test Effects

- Load a simple chase pattern or preset in FPP
- Verify smooth operation, no flickering, consistent brightness

---

## Phase 4: Power Injection Planning (1-2 hours)

### 4.1 Understand Voltage Drop

**Concept**: Electricity traveling through thin wire causes voltage drop. Long LED runs will have dimmer pixels at the far end if power is only fed from one end.

**Symptom**: Pixels at far end of string appear dim or off-color (shift toward red on white)

**Solution**: Inject power at multiple points along the string

### 4.2 Injection Point Calculation

**Rule of Thumb**:
- **5V LEDs**: Inject every 5 meters (every 50-100 pixels depending on density)
- **12V LEDs**: Inject every 10 meters (every 100-150 pixels)

**Your Zones**:

| Zone | Length | Pixel Count | Injection Points Needed | Notes |
|------|--------|-------------|------------------------|-------|
| Zone 1 | ___m | ___ px | Start + _________ | |
| Zone 2 | ___m | ___ px | Start + _________ | |
| Zone 3 | ___m | ___ px | Start + _________ | |

**Example**:
- Zone 1: 15m roofline, 150 pixels
- Injection: Start (0m) + Middle (7.5m) + End (15m) = 3 injection points

### 4.3 Wire Gauge Selection

| Run Length | Recommended AWG | Max Current |
|------------|-----------------|-------------|
| 0-3m | 22AWG | 5A |
| 3-6m | 18AWG | 10A |
| 6-10m | 16AWG | 13A |
| 10-15m | 14AWG | 17A |

**Recommendation**: Use 18AWG for most injection runs (good for up to 6m at full current)

### 4.4 Injection Wiring Diagram

**Home-Run Method** (recommended):
```
[12V PSU] ----→ [Distribution Block]
                      |
                      +---→ [Fuse 5A] → Zone 1 Start (V+ & GND only, not data)
                      |
                      +---→ [Fuse 5A] → Zone 1 Mid Point (V+ & GND splice)
                      |
                      +---→ [Fuse 5A] → Zone 1 End (V+ & GND splice)
                      |
                      +---→ [Fuse 5A] → Zone 2 Start
                      
(Data line runs only from PiCap to first pixel of each zone - no data injection)
```

**Important**: 
- **Never inject data at multiple points** (causes signal conflicts)
- Only inject **V+ and GND**
- Each injection uses 2-conductor wire (no need for 3rd conductor)

### 4.5 Fusing Strategy

**Why Fuse**: Protects wiring from overcurrent in case of short circuit

**Fuse Sizing**:
```
Pixels on segment × 0.05A = Fuse rating (round up to next standard size)

Examples:
- 100 pixels: 100 × 0.05A = 5A → Use 5A or 7.5A fuse
- 150 pixels: 150 × 0.05A = 7.5A → Use 7.5A or 10A fuse
```

**Falcon PiCap**: Has built-in fuses on outputs (check PiCap manual for ratings)

**Additional Fusing**: Add inline fuses on long power injection runs (>3m)

**Fuse Types**:
- Blade fuses (automotive type): Easy to source, inline holders available
- Glass fuses: Also common, cheaper
- Resettable PTC fuses: Auto-reset, but less precise

---

## Phase 5: Physical Installation (varies by scope)

### 5.1 Pre-Installation Planning

**Site Survey**:
1. Walk around house and verify mounting locations
2. Identify all cable routing paths
3. Locate all power injection points
4. Plan for controller/PSU enclosure location
5. Verify ladder access to all areas

**Safety Considerations**:
- ☐ Ladder safety: Helper to stabilize, appropriate ladder height
- ☐ Electrical safety: GFCI protection on AC side
- ☐ Weather: Do not install in rain, high wind, or extreme temperatures
- ☐ Fall protection: Consider harness for steep roofs
- ☐ Power tools: Proper grounding, extension cords rated for load

### 5.2 Install Controller Enclosure

**Location Requirements**:
- Protected from direct rain (under eave or in weatherproof box)
- Access to Ethernet and AC power
- Close to LED starting points (minimize long data runs)
- Temperature controlled if possible (avoid extreme heat/cold)

**Enclosure Setup**:
1. **Mount enclosure** to wall or secure surface
2. **Install cable glands** in enclosure for wire entry
3. **Mount Pi + PiCap** inside enclosure on standoffs or DIN rail
4. **Mount PSU** in separate enclosure or same enclosure if large enough
5. **Ensure ventilation** (some enclosures have vent ports)

**Wiring into Enclosure**:
- Run Ethernet through cable gland, connect to Pi
- Run power wires from PSU to PiCap input terminals
- Run output cables (data + power) to LED zones through cable glands
- **Strain relief**: Secure cables inside enclosure so no pull on terminals

### 5.3 Mount LED Pixels/Strips

**For Bullet Pixels on Roofline**:

1. **Install mounting track/clips**:
   - Boscoyo strips: Pre-drill holes every 30-60cm, screw to fascia
   - Gutter hooks: Clip onto gutter edge
   - Command strips: Clean surface, apply adhesive mounts

2. **Run pixel string along mounting**:
   - Start at controller end
   - Snap/clip each pixel into mounting point
   - Ensure consistent spacing
   - Keep wire slack to avoid tension

3. **Secure wiring**:
   - Use cable ties every 1-2 meters to secure wire to mounting or structure
   - Avoid tight bends in wire (minimum 2cm bend radius)

4. **Weatherproof connectors**:
   - Apply dielectric grease to all outdoor connectors before mating
   - Screw connectors tightly (Ray Wu connectors have threads)
   - Consider additional heat shrink or silicone over connectors for extra protection

**For LED Strips**:

1. **Mount aluminum channel**:
   - Cut channel to length with hacksaw
   - Pre-drill mounting holes every 30cm
   - Screw channel to mounting surface

2. **Install LED strip in channel**:
   - Peel backing if self-adhesive, press strip into channel
   - Or use silicone clips to secure strip in channel

3. **Install diffuser cover**:
   - Snap diffuser into channel (provides weatherproofing)

### 5.4 Run Power Injection Lines

1. **Plan wire routing** for each injection point (see 4.4 diagram)

2. **Run wires from PSU/distribution block to injection points**:
   - Use outdoor-rated wire or run in conduit
   - Secure with cable clips or ties every 30cm
   - Avoid contact with sharp edges

3. **Splice power into LED string at injection points**:
   - Cut into pixel string power wires (V+ and GND only)
   - Strip wires, connect injection wires using:
     - Waterproof wire nuts + heat shrink + silicone, OR
     - Solder + heat shrink + silicone, OR
     - Wago connectors in junction box
   - **Never cut data line** (data flows through from start to end)

4. **Install inline fuses** on injection runs:
   - Inline blade fuse holder on positive (+) wire
   - Mount fuse holder in accessible location for replacement

5. **Waterproof all splices**:
   - Heat shrink over solder joints
   - Silicone sealant over heat shrink
   - Or enclose splice in junction box with cable glands

### 5.5 Final Wiring Checks

**Visual Inspection**:
- ☐ All connectors mated correctly
- ☐ No exposed wire (all connections insulated)
- ☐ Strain relief on all cables
- ☐ Polarity correct on all connections (red to V+, black to GND)
- ☐ Data line intact from controller to end of each zone
- ☐ Weatherproofing applied to outdoor connections

**Continuity Testing** (with multimeter):
- ☐ V+ at PSU reaches all injection points (measure voltage)
- ☐ GND at PSU reaches all injection points
- ☐ No shorts between V+ and GND (infinite resistance)
- ☐ Data line continuous from PiCap output to first pixel

### 5.6 Power-On Test (Full Installation)

1. **Final check before power**:
   - Walk entire installation, verify all connections
   - Ensure no one touching wires or LEDs
   - Controller enclosure closed and secure

2. **Power on PSU**
   - Monitor for any smoking, burning smell, or unusual sounds
   - If any issues: **IMMEDIATELY POWER OFF**

3. **Access FPP and test each zone**:
   - Use Display Testing to light up Zone 1
   - Verify all pixels light up, no dim sections
   - Repeat for each zone

4. **Test full display**:
   - Run "All Lights On - White" test
   - Walk installation and verify consistent brightness
   - Check for any voltage drop issues (dim end pixels)

5. **Fine-tune injection if needed**:
   - If far-end pixels still dim, add additional injection point
   - Or use heavier gauge wire for injection runs

---

## Phase 6: Troubleshooting Common Issues

### Issue: LEDs Not Lighting

**Diagnosis Steps**:
1. Check power supply voltage (11.5-12.5V)
2. Verify V+ reaches LEDs (measure at first pixel with multimeter)
3. Check data line connection (continuous from PiCap to first pixel)
4. Verify FPP channel output configuration (pixel count, type, start channel)
5. Test with known-good LED string

**Common Causes**:
- No power to pixels
- Data line disconnected or wrong
- Wrong pixel type configured in FPP (WS2811 vs WS2812B)
- Reversed polarity

### Issue: Random Flickering or Wrong Colors

**Diagnosis Steps**:
1. Check for loose connections (data line especially)
2. Verify pixel type setting matches physical LEDs
3. Check for RF interference (nearby motors, radios)
4. Ensure adequate power supply capacity
5. Test with shorter string (eliminate wiring issues)

**Common Causes**:
- Data line poor connection
- Electrical noise on data line
- Wrong color order (RGB vs GRB) in FPP
- Insufficient power causing brownout

### Issue: Some Pixels Dim or Wrong Color at End of Run

**Diagnosis**: Voltage drop

**Solutions**:
- Add power injection point closer to dim section
- Use heavier gauge wire (upgrade 18AWG to 16AWG)
- Reduce overall brightness in FPP (90% max instead of 100%)

### Issue: Controller Won't Boot or FPP Not Accessible

**Diagnosis Steps**:
1. Check Pi power (red LED on Pi should be solid)
2. Verify SD card seated properly
3. Check Ethernet link LED (should blink on Pi and router)
4. Try accessing via IP address instead of `fpp.local`
5. Re-flash FPP image to SD card

**Common Causes**:
- SD card corrupted or not properly flashed
- Insufficient power to Pi (12V PSU too weak or regulator issue)
- Network configuration issue

### Issue: Fuse Keeps Blowing

**Diagnosis**: Overcurrent condition

**Causes**:
- Short circuit in wiring (V+ touching GND)
- Too many pixels on one fused output
- Damaged pixel drawing excessive current

**Solutions**:
- Check all wiring for shorts with multimeter
- Divide pixels into smaller groups with separate fuses
- Isolate damaged pixels (disconnect sections to find fault)

---

## Safety & Maintenance

### Electrical Safety

- **AC Side**:
  - All AC wiring by qualified electrician or follow local codes
  - GFCI protection required for outdoor outlets
  - Proper grounding of PSU and enclosures

- **DC Side** (12V):
  - Generally low voltage and safer, but still follow good practices
  - Fuse all outputs appropriately
  - Avoid creating short circuits (can cause fires even at 12V with high current)

### Weather Protection

- **IP Ratings**:
  - Controller enclosure: IP65 minimum (dust tight, water jets)
  - LEDs: IP67 minimum (immersion to 1m for 30 minutes)
  - Connectors: Outdoor-rated, dielectric grease applied

- **Seasonal Checks**:
  - Before each holiday season, inspect all connections
  - Look for corrosion, damaged insulation, loose connections
  - Test a few random sections before full power-on

### Routine Maintenance

**Monthly** (during active use):
- Visual inspection of installation
- Check enclosure seals
- Verify no water intrusion

**Annually**:
- Full connection inspection and re-tightening
- Re-apply dielectric grease to connectors
- Test all fuses (replace any that look corroded)
- Clean dust from controller enclosure
- Update FPP software

**After Storms**:
- Inspect for physical damage (wind, hail)
- Check for water intrusion
- Test all zones before next use

---

## Documentation & Labeling

**Label Everything**:
- Each zone output at controller: "Zone 1 - Front Roof"
- Each PSU output: "Zone 1 & 2 Power"
- Each fuse: "Zone 1 Mid-point Injection - 5A"
- Each enclosure: "LED Controller - 12V DC" (warning labels)

**Create Wiring Diagram**:
- Draw (or photograph) as-built wiring
- Note wire gauges, fuse ratings, pixel counts
- Store in waterproof document holder attached to controller enclosure

**Spare Parts Kit**:
- 10x extra pixels (for repairs)
- 2x extra connectors
- 2x fuses of each rating used
- 2m spare 18AWG 3-conductor wire
- Heat shrink tubing assortment
- Dielectric grease tube

---

## Next Steps

After hardware is installed and tested:

1. **Proceed to [software_setup.md](./software_setup.md)** for FPP detailed configuration
2. **Set up Home Assistant integration** per [home_assistant_integration.md](./home_assistant_integration.md)
3. **Start creating sequences** using xLights (tutorials linked in software guide)

---

**Updated**: 2025-12-24
