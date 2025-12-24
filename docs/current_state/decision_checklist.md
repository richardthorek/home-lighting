# Critical Decision Checklist - Home LED Lighting Project

## Overview

This checklist guides you through all critical decisions needed before ordering parts and starting installation. Each section presents options with pros, cons, and recommendations based on common use cases.

**How to Use This Checklist**:
1. Read each decision point carefully
2. Check (✅) your chosen option
3. Note any specific details in the "Your Choice" column
4. Review dependencies between decisions
5. Validate your choices against budget and goals

---

## Decision 1: Controller Platform

**Question**: Which controller hardware should I use?

| Option | Pros | Cons | Best For | Your Choice |
|--------|------|------|----------|-------------|
| **Falcon PiCap + Raspberry Pi** ⭐ | • Scalable (2-4 outputs, 800px each)<br>• Wired Ethernet reliability<br>• FPP software full-featured<br>• Community support excellent | • Higher cost ($100+)<br>• More complex setup<br>• Requires Ethernet access | • Permanent outdoor installations<br>• 300+ pixels<br>• Future expansion plans | ✅ |
| **WLED on ESP32** | • Very low cost ($10-30)<br>• Simple setup<br>• Rich built-in effects<br>• Wi-Fi control | • Wi-Fi reliability concerns<br>• Limited to few hundred pixels<br>• Multiple controllers for large setups | • Indoor installations<br>• Smaller displays (< 300 pixels)<br>• Quick DIY projects | ☐ |
| **Falcon F16v4** | • 16 outputs (expandable to 48)<br>• Massive capacity<br>• Pro-grade reliability | • Expensive ($240+)<br>• Overkill for small setups | • Very large displays (5000+ pixels)<br>• Multiple zones<br>• Professional installations | ☐ |
| **Hanson rPi-28D+** (AU) | • Cost effective ($38 AUD)<br>• 2 outputs<br>• FPP compatible | • Limited outputs<br>• Less documentation vs Falcon | • Australia buyers<br>• Budget-conscious<br>• Similar to PiCap scope | ☐ |

**Recommendation**: **Falcon PiCap + Raspberry Pi** for the target use case (permanent, scalable, HA integrated)

**Dependencies**: 
- This choice affects software setup (FPP vs WLED)
- Impacts wiring approach (centralized vs distributed)
- Influences network requirements (Ethernet vs Wi-Fi)

**Your Decision**: **Falcon PiCap + Raspberry Pi** - Chosen for scalability, reliability, and wired Ethernet control

---

## Decision 2: LED Type & Voltage

**Question**: What type of addressable LEDs should I use?

### Voltage Choice

| Voltage | Pros | Cons | Recommended For | Your Choice |
|---------|------|------|-----------------|-------------|
| **12V** ⭐ | • Less voltage drop (longer runs)<br>• Fewer power injection points<br>• Better for outdoor/long distances | • Slightly higher component cost<br>• Less common for indoor use | • Rooflines (10m+ runs)<br>• Outdoor permanent installs<br>• Large displays | ✅ |
| **5V** | • Widely available<br>• Slightly cheaper per pixel<br>• More granular control options | • More power injection needed<br>• Voltage drop limits runs to ~5m<br>• Higher current draw | • Indoor installations<br>• Props and close-up viewing<br>• High-density effects | ☐ |

### Chip Type (assuming 12V chosen)

| Chip | Control Granularity | Pros | Cons | Cost | Your Choice |
|------|-------------------|------|------|------|-------------|
| **WS2811** ⭐ | Groups of 3 LEDs | • Industry standard outdoor<br>• Proven reliability<br>• Widely available<br>• Good price | • Less granular (3-LED groups) | $0.25-0.40/px | ✅ |
| **WS2815** | Individual LEDs | • Individual LED control<br>• Backup data line<br>• 12V benefits | • More expensive<br>• Less common | $0.45-0.70/px | ☐ |

**Recommendation**: **12V WS2811** for rooflines, consider **12V WS2815** for close-up viewing areas if budget allows

**Your Decision**: **12V WS2811** - Industry standard for outdoor installations, good balance of cost and performance

---

## Decision 3: LED Form Factor

**Question**: Bullet pixels or LED strips?

| Form Factor | Description | Pros | Cons | Best Use | Your Choice |
|-------------|-------------|------|------|----------|-------------|
| **Bullet Pixels (Nodes)** ⭐ | Individual sealed LED modules on wire, typically 10cm spacing | • Easy to repair (replace single node)<br>• Weatherproof (IP67/68)<br>• Flexible routing<br>• Clean outline look | • More visible as individual points<br>• Takes longer to install many | • Rooflines<br>• House outlines<br>• Windows/doors<br>• Tree wrapping | ✅ |
| **LED Strip** | Continuous flexible strip with LEDs | • Continuous light line<br>• Faster installation for straight runs<br>• Can be hidden in channels | • Harder to repair (section replacement)<br>• Less flexible routing<br>• Weatherproofing more critical | • Under eaves (hidden)<br>• Architectural accents<br>• Fill areas<br>• Smooth light washes | ☐ |
| **Both (Hybrid)** | Mix of bullets and strips | • Use right tool for each area<br>• Maximum flexibility | • More complex planning<br>• Two different mounting methods | • Large complex displays<br>• Varied architectural features | ☐ |

**Recommendation**: **Bullet Pixels** for primary roofline work, add strips later for fill areas if needed

**Your Decision**: **Bullet Pixels (Nodes)** - Starting with bullet pixels, likely to add LED strips in the future for fill areas and continuous lines

---

## Decision 4: Pixel Density

**Question**: What spacing between pixels?

| Spacing | Pixels per Meter | Visual Effect | Power Draw (12V WS2811) | Cost Impact | Your Choice |
|---------|------------------|---------------|------------------------|-------------|-------------|
| **15cm (6")** | 6.7 px/m | Sparse, individual points visible | Low | Budget-friendly | ☐ |
| **10cm (4")** ⭐ | 10 px/m | Balanced - good for rooflines | Medium | Standard pricing | ✅ |
| **7.5cm (3")** | 13.3 px/m | Dense, smoother animations | Medium-high | Moderate premium | ☐ |
| **5cm (2")** | 20 px/m | Very dense, nearly continuous | High | Expensive | ☐ |

**Calculation Example**:
```
10m roofline × 10 pixels/m = 100 pixels
10m roofline × 13.3 pixels/m = 133 pixels (+33% cost)
10m roofline × 20 pixels/m = 200 pixels (+100% cost)
```

**Recommendation**: **10cm (4")** spacing for rooflines - good balance of effect and cost

**Considerations**:
- Viewing distance: Further away = can use sparser spacing
- Effect type: Chasing patterns need denser pixels for smooth look
- Budget: Denser = more pixels = higher cost and power

**Your Decision**: **10cm (4")** - Starting with 10cm spacing, planning to mix densities over time as different zones are added

---

## Decision 5: Total Pixel Count & Zones

**Question**: How many pixels do I need, and in how many zones?

### Step 1: Measure Your Installation Areas

| Area | Length (meters) | Spacing Choice | Pixels Needed | Notes |
|------|----------------|----------------|---------------|-------|
| Front roofline | _____ m | _____ cm | _____ px | |
| Garage roofline | _____ m | _____ cm | _____ px | |
| Side eaves | _____ m | _____ cm | _____ px | |
| Window outlines | _____ m | _____ cm | _____ px | |
| Other: _______ | _____ m | _____ cm | _____ px | |
| **TOTAL** | **_____ m** | | **_____ px** | |

### Step 2: Group into Control Zones

| Zone # | Description | Pixel Count | FPP Output | Your Notes |
|--------|-------------|-------------|------------|------------|
| Zone 1 | ________________ | _____ px | Output 1 | |
| Zone 2 | ________________ | _____ px | Output 2 | |
| Zone 3 | ________________ | _____ px | Output 3 | |
| Zone 4 | ________________ | _____ px | Output 4 | |

**Constraint**: Falcon PiCap outputs support ~800 pixels each max

**Recommendation**: Start with 2 zones (300-500 pixels total), expand after validation

**Your Decision**: **500 pixels** - Starting with 500 pixels across 2 zones, specific zone allocation TBD during installation planning

---

## Decision 6: Power Supply Sizing

**Question**: What size power supply do I need?

### Power Calculation

**Formula**: `Pixels × 0.05A (for 12V) = Max Amps Needed`

| Your Pixel Count | Max Amps (100% white) | Recommended PSU | Wattage | Your Choice |
|------------------|----------------------|-----------------|---------|-------------|
| 100 pixels | 5A | Mean Well LRS-100-12 | 100W | ☐ |
| 300 pixels | 15A | Mean Well LRS-200-12 | 200W | ☐ |
| 500 pixels ⭐ | 25A | Mean Well LRS-350-12 | 350W | ✅ |
| 800 pixels | 40A | Mean Well LRS-350-12 × 2 | 700W | ☐ |
| 1000 pixels | 50A | Mean Well LRS-600-12 | 600W | ☐ |
| 1500 pixels | 75A | Mean Well LRS-600-12 + LRS-350-12 | 950W | ☐ |

**Notes**:
- **Always add 20-30% headroom** for safety and longevity
- Most displays run at 40-60% brightness on average, not 100%
- Multiple smaller PSUs can be better than one huge PSU (redundancy, distribution)

**Recommendation**: Start with **Mean Well LRS-350-12** (350W) for 300-500 pixel range

**Your Decision**: **Mean Well LRS-350-12 (350W)** - Sized for 500 pixels with 20-30% headroom for safety

---

## Decision 7: Power Injection Strategy

**Question**: Where and how often should I inject power?

### Power Injection Guidelines

| LED Type | Max Run Without Injection | Injection Recommendation | Your Choice |
|----------|--------------------------|-------------------------|-------------|
| **5V LEDs** | ~5m | Every 5m (both ends of 5m sections) | ☐ |
| **12V LEDs** ⭐ | ~10m | Both ends of 10m runs minimum | ✅ |
| **12V LEDs (high density)** | ~7m | Every 7-8m for 20px/m density | ☐ |

### Your Injection Plan

| Zone | Total Length | Injection Points | Wire Gauge | Notes |
|------|--------------|------------------|------------|-------|
| Zone 1 | _____ m | Start + _____ | 18 or 16 AWG | |
| Zone 2 | _____ m | Start + _____ | 18 or 16 AWG | |
| Zone 3 | _____ m | Start + _____ | 18 or 16 AWG | |

**Approach Options**:
- **Home-run injection** ⭐: Separate wire from PSU to each injection point (best voltage stability)
- **Daisy-chain injection**: Feed through one section to next (simpler but more voltage drop)

**Recommendation**: **Home-run injection** for runs over 10m, daisy-chain acceptable for shorter runs

**Your Decision**: **Both ends strategy** - Using 12V allows injection at start and end of runs. Will use home-run injection from PSU to ensure best voltage stability

---

## Decision 8: Controller & PSU Location

**Question**: Where will the controller and power supply be mounted?

### Location Considerations

| Factor | Indoor | Outdoor (Enclosure) | Your Choice |
|--------|--------|---------------------|-------------|
| **Ease of Access** | Easy (for troubleshooting) | Harder (weatherproof entry) | ☐ Indoor / ☐ Outdoor |
| **Wiring Complexity** | More outdoor wiring needed | Shorter runs to LEDs | |
| **Weatherproofing** | Not needed for controller | Must use IP65+ enclosures | |
| **Ethernet Access** | Usually easy | Need outdoor-rated cable or conduit | |
| **Power Access** | Standard outlets | Need outdoor outlet or extension | |

**Recommendation**: 
- **PSU**: Indoors if possible (garages, utility rooms)
- **Controller**: Near PSU location, but Ethernet access critical

### Your Location Plan:
- **Controller Location**: Under cover at edge of house with ethernet access to home network switch
- **PSU Location**: TBD - likely near controller location
- **Distance to LED start point**: TBD - will be measured during installation planning
- **Ethernet run length**: TBD - will be measured based on final controller placement

**Your Decision**: **Controller under cover at edge of house** - Protected from weather, with direct ethernet connection to home switch. Final placement TBD during installation phase

---

## Decision 9: Mounting Method

**Question**: How will I mount the LEDs?

### For Bullet Pixels

| Method | Pros | Cons | Cost | Your Choice |
|--------|------|------|------|-------------|
| **Boscoyo Mounting Strips** ⭐ | • Clean, professional<br>• Pre-drilled holes<br>• Easy pixel replacement | • Must purchase specialty product<br>• Moderate cost | $2-3/ft | ☐ |
| **Gutter/Shingle Clips** | • Simple, removable<br>• No drilling into house | • Less stable in wind<br>• Visible clips | $0.20/ea | ☐ |
| **Hot Glue (temp)** | • Very fast<br>• Cheap<br>• No holes | • Not permanent<br>• Removal can be messy | Minimal | ☐ |
| **3M Command Strips** | • Removable<br>• No damage to surfaces | • Can fail in extreme temp<br>• Moderate cost | $0.40-0.60/strip | ☐ |
| **Drill + Zip Ties** | • Very secure<br>• Cheap after drill | • Permanent holes<br>• Labor intensive | Drill + ties | ☐ |

### For LED Strips

| Method | Pros | Cons | Cost | Your Choice |
|--------|------|------|------|-------------|
| **Aluminum Channel + Diffuser** ⭐ | • Professional look<br>• Protects strip<br>• Diffused light | • Moderate cost<br>• Need to cut to length | $5-10/m | ☐ |
| **Adhesive Backing (built-in)** | • Very fast install<br>• Clean look | • Adhesive can fail outdoors<br>• Hard to reposition | Included w/ strip | ☐ |
| **Silicone Clips** | • Secure<br>• Flexible positioning | • More visible<br>• Slower install | $0.10/clip | ☐ |

**Recommendation**: **Boscoyo strips** for bullet pixels, **Aluminum channels** for strips

**Your Decision**: **TBD** - Will be determined during installation planning phase based on mounting surfaces and final budget

---

## Decision 10: Network Architecture

**Question**: How will the controller connect to the network?

| Method | Pros | Cons | Recommended For | Your Choice |
|--------|------|------|-----------------|-------------|
| **Wired Ethernet** ⭐ | • Most reliable<br>• No interference<br>• Lowest latency | • Must run Cat5/6 cable<br>• Drill/conduit may be needed | • Permanent installations<br>• Outdoor controllers<br>• Falcon/FPP setups | ✅ |
| **Wi-Fi** | • No cable run needed<br>• Easier initial setup | • Can be unreliable outdoors<br>• Subject to interference<br>• Range limitations | • Indoor controllers<br>• WLED setups<br>• Temporary displays | ☐ |
| **Powerline Ethernet** | • Uses existing AC wiring<br>• No new cables | • Can be unreliable<br>• Latency variability | • When ethernet impossible<br>• Indoor only | ☐ |

**Your Plan**:
- **Connection Method**: Wired Ethernet initially, with consideration for wireless connectivity to some elements in the future
- **Router to Controller Distance**: TBD - will be measured during installation planning
- **Cable route plan**: TBD - will be planned to minimize exposed cable runs

**Recommendation**: **Wired Ethernet** for the Falcon PiCap setup (reliability is critical)

**Your Decision**: **Wired Ethernet** - Primary controller will use wired ethernet for reliability. May add wireless connectivity for future expansion elements

---

## Decision 11: Home Assistant Integration

**Question**: How will I integrate with Home Assistant?

| Integration Method | Complexity | Features | Best For | Your Choice |
|-------------------|------------|----------|----------|-------------|
| **FPP MQTT Plugin** ⭐ | Medium | • Native HA light entities<br>• Color/brightness control<br>• Auto-discovery | • FPP/Falcon setups<br>• Daily control + show capability | ✅ |
| **WLED Integration** | Low | • Auto-discovery<br>• Rich effects library<br>• Segments support | • WLED controllers<br>• Simplified setups | ☐ |
| **HTTP API Calls** | High | • Full control possible<br>• Custom automations | • Advanced users<br>• Custom workflows | ☐ |
| **Node-RED Bridge** | Medium-High | • Visual programming<br>• Complex logic | • Users already using Node-RED | ☐ |

**Prerequisites for FPP MQTT**:
- MQTT broker (Mosquitto) installed on HA
- FPP overlay models defined
- FPP HA plugin enabled

**Recommendation**: **FPP MQTT Plugin** (aligns with Falcon PiCap architecture choice)

**Your Decision**: **FPP MQTT Plugin** - Native integration with Home Assistant using MQTT for reliable control

---

## Decision 12: Sequence Creation Approach

**Question**: How will I create lighting effects and sequences?

| Approach | Effort | Quality | Cost | Your Choice |
|----------|--------|---------|------|-------------|
| **Learn xLights** ⭐ | High initial | Professional results | Free | ☐ |
| **Use Pre-made Sequences** | Low | Good (community quality) | Free-$50/sequence | ☐ |
| **Simple FPP Scripts** | Medium | Basic effects | Free | ☐ |
| **Home Assistant Only (no sequences)** | Low | Very basic (solid colors) | Free | ☐ |
| **Hire Sequence Designer** | Low for you | Professional | $200-1000+ | ☐ |

**Recommendation**: 
- Start with **HA solid colors** for daily use
- Learn **xLights basics** for simple chasing/fading effects
- Add **pre-made sequences** for holidays
- Invest time in **custom xLights** as skills grow

**Your Plan**: TBD - Will be determined after test bench setup and initial experimentation

**Your Decision**: **TBD** - Sequence creation approach to be determined based on experience with xLights and time available

---

## Decision 13: Expansion Path

**Question**: How might I expand this system in the future?

### Future Expansion Options

| Expansion | Requirements | Estimated Cost | Timeline | Your Interest |
|-----------|-------------|----------------|----------|---------------|
| **Add More Zones** | More pixels, possibly another PSU | $200-500 | Phase 2 | ☐ Yes / ☐ No |
| **Second Controller** | Another Pi+PiCap or F16 | $100-250 | Phase 3 | ☐ Yes / ☐ No |
| **DMX Fixtures** | DMX lights, FPP already supports | $50-300/fixture | Future | ☐ Yes / ☐ No |
| **Props (Trees, Arches)** | More pixels, prop designs | $100-500/prop | Future | ☐ Yes / ☐ No |
| **Music Sync** (FM transmitter) | FM transmitter kit | $50-100 | Phase 2 | ☐ Yes / ☐ No |

**Design Consideration**: Falcon PiCap can expand to 4 outputs (~3200 pixels max). After that, would need second controller or upgrade to F16.

**Your Expansion Thoughts**: **TBD** - Will be determined after initial installation is operational and performance is evaluated

---

## Decision 14: Budget Allocation

**Question**: What's my budget breakdown?

### Budget Planning

| Category | Minimum | Recommended | Premium | Your Budget |
|----------|---------|-------------|---------|-------------|
| **Controller** (Pi + PiCap) | $100 | $100 | $100 | $_____ |
| **Power Supply** | $30 | $50 | $80 | $_____ |
| **LEDs** (500px) | $125 | $175 | $250 | $_____ |
| **Wiring & Connectors** | $50 | $100 | $150 | $_____ |
| **Mounting Hardware** | $30 | $80 | $150 | $_____ |
| **Enclosures** | $30 | $60 | $100 | $_____ |
| **Tools** (if needed) | $50 | $100 | $200 | $_____ |
| **Misc/Contingency** | $50 | $100 | $150 | $_____ |
| **TOTAL** | **$465** | **$765** | **$1180** | **TBD** |

**Your Total Budget**: TBD - Will be finalized after initial phase costs are confirmed

**Funding Approach**:
- ☐ All at once
- ☐ Phase 1 budget: $_____, Phase 2 budget: $_____
- ☐ Buy controller + test first, then expand

**Your Decision**: **TBD** - Budget allocation to be determined during procurement planning

---

## Decision 15: Timeline & Commitment

**Question**: What's my realistic timeline?

| Phase | Duration | Your Target Date | Your Commitment Level |
|-------|----------|------------------|-----------------------|
| **Planning & Ordering** | 1-2 weeks | TBD | ☐ High ☐ Medium ☐ Low |
| **Parts Arrival** | 1-4 weeks | TBD | (waiting) |
| **Test Bench Setup** | 1 week | TBD | ☐ High ☐ Medium ☐ Low |
| **HA Integration** | 1 week | TBD | ☐ High ☐ Medium ☐ Low |
| **Physical Installation** | 2-3 weeks | TBD | ☐ High ☐ Medium ☐ Low |
| **Sequence Creation** | Ongoing | TBD | ☐ High ☐ Medium ☐ Low |

**Realistic Availability**:
- Hours per week for this project: TBD
- Preferred work days: TBD
- Deadline (if any): TBD

**Key Constraint**: Weather for outdoor installation

**Your Decision**: **TBD** - Timeline and commitment levels to be determined during planning phase

---

## Final Validation Checklist

Before ordering, confirm:

- ☐ All 15 decision points reviewed and documented
- ☐ Measurements taken for LED runs
- ☐ Pixel counts calculated
- ☐ Power supply sized appropriately (+20% headroom)
- ☐ Budget allocated and approved
- ☐ Controller location identified with Ethernet access
- ☐ Mounting method chosen and hardware identified
- ☐ Shopping list finalized with quantities
- ☐ Supplier links verified and in-stock checked
- ☐ Timeline realistic given availability
- ☐ Family/household on board with project
- ☐ Safety considerations reviewed (electrical, ladder work)
- ☐ Tools inventory - have what I need or ordered
- ☐ Backup plan for if parts arrive damaged/wrong

---

## Your Decision Summary

**Controller**: Falcon PiCap + Raspberry Pi  
**LEDs**: 12V WS2811 Bullet Pixels, 10cm (4") spacing  
**Total Pixels**: 500 pixels (starting point)  
**Zones**: 2 zones (specific allocation TBD)  
**Power Supply**: Mean Well LRS-350-12 (350W)  
**Mounting**: TBD (during installation planning)  
**Integration**: FPP MQTT Plugin for Home Assistant  
**Budget**: TBD  
**Timeline**: Start TBD, Complete TBD  

**Next Step**: Proceed to [shopping_list.md](./shopping_list.md) and order your parts!

---

**Updated**: 2025-12-24
