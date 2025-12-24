# Home Christmas Light Project - Master Plan

## Executive Summary

This master plan serves as the definitive implementation guide for a scalable, year-round addressable LED lighting system for the home, with a focus on Christmas and holiday displays. The solution leverages **Falcon PiCap** controller hardware mounted on a **Raspberry Pi** running **Falcon Player (FPP)**, integrated with **Home Assistant** for daily control and automation.

**Key Design Decisions:**
- **Hardware Platform**: Falcon PiCap + Raspberry Pi 3/4 (chosen for scalability, reliability, wired Ethernet control)
- **LED Type**: 12V WS2811 addressable LED pixels/strips (optimal for long outdoor runs with less power injection)
- **Control Software**: Falcon Player (FPP) for show management, xLights for sequencing, Home Assistant for daily automation
- **Integration**: Home Assistant MQTT plugin for seamless smart home control
- **Network**: Wired Ethernet for controller reliability

**Current Phase**: Planning and documentation (as of 2025-12-24)

**Target Use Cases**:
1. Daily ambient lighting with simple colors/effects via Home Assistant
2. Holiday-specific displays (Christmas, Halloween, etc.) with synchronized music shows
3. Event-based lighting (birthdays, celebrations) with custom sequences
4. Year-round architectural accent lighting

---

## Architecture Overview

### System Architecture Description (Prompt Form)

**For Gemini Nano Banana Pro (Image Generation):**
> Create a detailed architectural diagram showing a home Christmas lighting system. The diagram should include: (1) A house outline with LED strips mounted along rooflines and eaves, showing 2-3 distinct zones; (2) A central control enclosure containing a Raspberry Pi with Falcon PiCap HAT, connected to a 12V power supply (350W); (3) Network connectivity showing the Pi connected via Ethernet cable to a home router; (4) The router connected to a Home Assistant server; (5) Power injection points marked at the start and end of LED runs; (6) Arrows showing data flow (Ethernet) in blue and power flow in red; (7) A smartphone/tablet showing the Home Assistant interface for controlling lights; (8) Labels for all major components: "Falcon PiCap on Raspberry Pi", "FPP Software", "12V WS2811 LED Pixels", "Power Supply (350W)", "Home Assistant Dashboard", "MQTT Broker". Use a clean, technical illustration style with a slight 3D perspective.

**For GitHub Copilot (Mermaid Diagram):**
```markdown
Create a Mermaid diagram showing the architecture of a home LED lighting system with the following components and relationships:

1. Home Assistant (central hub) connected via MQTT to Raspberry Pi running FPP
2. Raspberry Pi with Falcon PiCap HAT outputting to multiple LED zones
3. Power supply feeding the Falcon PiCap and LED strings
4. Network router connecting Home Assistant to the Pi via Ethernet
5. xLights software (on PC) sending sequences to FPP via network
6. LED Zones: Front Roofline (Zone 1), Side Eaves (Zone 2), Garage Outline (Zone 3)
7. Show data flow, power flow, and control relationships

Use a graph TD (top-down) format with appropriate styling to distinguish between:
- Control signals (blue)
- Power flow (red)
- Network communication (green)
- Physical devices (boxes with distinct colors)
```

### Detailed Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         Home Network                             │
│  ┌──────────────┐          Ethernet        ┌──────────────┐    │
│  │    Router    │◄────────────────────────►│ Home Asst.   │    │
│  │              │                           │   Server     │    │
│  └──────┬───────┘                           └──────────────┘    │
│         │ Ethernet                                               │
│         │                                                        │
│  ┌──────▼────────────────────────┐                              │
│  │  Raspberry Pi 3/4             │                              │
│  │  - FPP Software               │                              │
│  │  - MQTT Client                │                              │
│  │  ┌─────────────────────────┐  │                              │
│  │  │   Falcon PiCap HAT     │  │                              │
│  │  │   - Output 1 (800 px)  │──┼──► Zone 1: Front Roofline    │
│  │  │   - Output 2 (800 px)  │──┼──► Zone 2: Side Eaves        │
│  │  └─────────────────────────┘  │                              │
│  └───────────▲───────────────────┘                              │
│              │ 12V Power                                         │
│  ┌───────────┴───────────────┐                                  │
│  │  12V Power Supply (350W)  │                                  │
│  │  Mean Well or equivalent  │                                  │
│  └───────────────────────────┘                                  │
└─────────────────────────────────────────────────────────────────┘

External Tools (Development):
┌──────────────┐
│   xLights    │──► Sequence Creation (FSEQ files) ──► FPP
│   (PC/Mac)   │    via network transfer
└──────────────┘
```

**Component Interactions:**
1. **Home Assistant → FPP**: MQTT commands for on/off, color changes, brightness
2. **FPP → Falcon PiCap**: Direct GPIO communication for pixel data
3. **Falcon PiCap → LEDs**: WS2811 protocol data signal + power
4. **xLights → FPP**: Sequence file uploads and playback triggering
5. **Power Supply → PiCap + LEDs**: 12V DC power with fused distribution

---

## Project Phases

### Phase 1: Planning & Documentation (Current - Week of 2025-12-24)
**Status**: ✅ In Progress  
**Duration**: 1 week

**Objectives**:
- ✅ Create comprehensive master plan and documentation
- ✅ Define hardware architecture and component selection
- ✅ Produce actionable shopping list with comparative analysis
- ✅ Document decision criteria and trade-offs
- ✅ Establish documentation structure for ongoing updates

**Deliverables**:
- master_plan.md (this document)
- docs/current_state/ with all implementation guides
- .github/copilot-instructions.md
- Architecture diagram prompts
- Complete shopping list with supplier links

**Blockers/Risks**: None identified

---

### Phase 2: Hardware Procurement (Week of 2025-12-31)
**Status**: ⏳ Not Started  
**Duration**: 1-2 weeks (depends on shipping)

**Objectives**:
- Order Falcon PiCap, Raspberry Pi, and essential components
- Order initial LED test batch (50-100 pixels)
- Order power supply and connectors
- Receive and inventory all components

**Deliverables**:
- All hardware components received and inventoried
- Bill of materials with actual costs documented
- Photos of components for documentation

**Prerequisites**:
- Shopping list finalized (Phase 1)
- Budget approved
- Supplier availability confirmed

**Blockers/Risks**:
- Shipping delays (especially for international orders)
- Component availability (Falcon PiCap stock)
- Budget constraints

---

### Phase 3: Test Bench Setup (Week of 2026-01-06)
**Status**: ⏳ Not Started  
**Duration**: 1 week

**Objectives**:
- Assemble Raspberry Pi + Falcon PiCap on test bench
- Install and configure FPP software
- Connect test LED string (50-100 pixels)
- Verify power supply and data signal integrity
- Configure basic FPP output settings
- Test pattern generation and LED control

**Deliverables**:
- Working test bench with 50-100 test LEDs
- FPP configured and accessible via web UI
- Documentation of test results with photos/videos
- Troubleshooting notes for common issues

**Prerequisites**:
- All Phase 2 hardware received
- Network connectivity available for Pi
- Basic workbench setup with tools

**Blockers/Risks**:
- Hardware compatibility issues
- FPP configuration challenges
- Power supply issues or incorrect wiring

---

### Phase 4: Home Assistant Integration (Week of 2026-01-13)
**Status**: ⏳ Not Started  
**Duration**: 1 week

**Objectives**:
- Set up MQTT broker (Mosquitto) on Home Assistant
- Install FPP Home Assistant MQTT plugin
- Configure FPP overlay models for LED zones
- Create Home Assistant automations for daily lighting
- Test on/off, color, brightness control from HA
- Document integration configuration

**Deliverables**:
- Working Home Assistant → FPP → LED control chain
- LED zones appearing as light entities in HA
- Basic automations (sunset on, sunrise off, holiday themes)
- Updated haconfiguration repo with configs
- Integration guide in docs/current_state/

**Prerequisites**:
- Phase 3 test bench operational
- Home Assistant instance accessible
- MQTT broker knowledge or tutorials referenced

**Blockers/Risks**:
- MQTT configuration complexity
- Network connectivity issues
- FPP plugin compatibility with HA version

---

### Phase 5: Physical Installation (Week of 2026-01-20)
**Status**: ⏳ Not Started  
**Duration**: 2-3 weeks

**Objectives**:
- Plan mounting locations and LED run paths
- Install mounting hardware (channels, clips, or tracks)
- Run LED strings along rooflines and eaves
- Implement power injection at strategic points
- Install weatherproof enclosure for controller and PSU
- Test full installation for electrical safety and functionality

**Deliverables**:
- Complete LED installation on house exterior
- Controller and PSU in weatherproof housing
- All wiring secured and weatherproofed
- Installation photos and notes
- Electrical safety verification checklist

**Prerequisites**:
- Weather suitable for outdoor work
- All mounting hardware acquired
- Ladder and installation tools available
- Helper for safety (recommended for roofline work)

**Blockers/Risks**:
- Weather delays (rain, cold, wind)
- Physical installation challenges (roof access, mounting surface issues)
- Cable length miscalculations requiring additional parts
- Electrical safety concerns

---

### Phase 6: Sequence Creation & Testing (Week of 2026-02-10)
**Status**: ⏳ Not Started  
**Duration**: 2-4 weeks (ongoing)

**Objectives**:
- Install and learn xLights software
- Create house model in xLights matching physical layout
- Design basic effects sequences (chasing lights, color fades, etc.)
- Create holiday-specific sequences (Christmas, Halloween, etc.)
- Upload sequences to FPP and test playback
- Schedule automated show times in FPP

**Deliverables**:
- xLights house model file
- Library of 5-10 base sequences
- FPP playlists for different occasions
- Scheduling configuration for automated shows
- Sequence creation guide/notes

**Prerequisites**:
- Phase 5 physical installation complete
- PC/Mac with xLights installed
- Understanding of xLights basics (tutorials referenced)
- Music files for synchronized shows (if desired)

**Blockers/Risks**:
- xLights learning curve
- Sequence creation time (can be very time-consuming)
- Performance issues with large pixel counts
- Creative block or perfectionism delaying completion

---

### Phase 7: Refinement & Expansion (Ongoing from 2026-03)
**Status**: ⏳ Not Started  
**Duration**: Ongoing

**Objectives**:
- Monitor system performance and reliability
- Address any issues or failures
- Expand to additional zones as desired
- Optimize power consumption and network performance
- Create additional seasonal sequences
- Share knowledge with community

**Deliverables**:
- Performance monitoring logs
- System reliability metrics
- Expansion documentation (if applicable)
- Lessons learned document
- Community contributions (blog posts, forum help, etc.)

**Prerequisites**:
- All previous phases complete
- System running for at least one full holiday season
- Budget for expansions (if desired)

**Blockers/Risks**:
- Component failures requiring replacement
- Weather damage to installation
- Scope creep (always wanting to add more!)

---

## Decision Log

### Entry 1: 2025-12-24 - Initial Planning Phase
**Decision**: Adopt Falcon PiCap + Raspberry Pi architecture over WLED/ESP32 approach

**Rationale**:
- **Scalability**: PiCap provides path to expansion (up to 4 outputs, ~800 pixels each) without managing multiple Wi-Fi controllers
- **Reliability**: Wired Ethernet eliminates Wi-Fi connectivity issues outdoors
- **Professional-grade**: Falcon hardware is industry-standard in holiday lighting community
- **Future-proofing**: Foundation for complex synchronized shows if desired later
- **Centralized control**: Single controller simplifies management and synchronization

**Trade-offs Accepted**:
- Higher upfront cost (~$100 for Pi+PiCap vs ~$20 for ESP32)
- Steeper learning curve (FPP + xLights vs simple WLED interface)
- More complex initial setup
- Wired Ethernet requirement for controller location

**Alternatives Considered**:
1. **WLED on ESP32**: Rejected due to Wi-Fi reliability concerns for permanent outdoor install and limited scalability
2. **Falcon F16v4**: Rejected as overkill for current scope (16 outputs, $240 cost)
3. **Kulp K8-PB**: Considered but Falcon has better documentation and community support

**Next Steps**:
- Proceed with detailed hardware shopping list for Falcon PiCap
- Research FPP installation and configuration tutorials
- Plan network connectivity to controller location

---

### Entry 2: 2025-12-24 - LED Type Selection
**Decision**: Use 12V WS2811 addressable LED pixels for primary installation

**Rationale**:
- **Voltage drop management**: 12V allows 10m runs with injection only at ends (vs 5V requiring mid-run injection every 5m)
- **Simplified wiring**: Fewer injection points reduces installation complexity
- **Outdoor reliability**: 12V pixels with IP67/IP68 rating are standard for permanent outdoor installations
- **Community proven**: Most large holiday displays use 12V WS2811 for rooflines
- **Power efficiency**: Lower current draw at higher voltage reduces resistive losses

**Trade-offs Accepted**:
- **Granularity**: WS2811 controls LEDs in groups of 3 (3 LEDs = 1 pixel), less precise than WS2812B individual control
- **Cost**: Slightly more expensive than 5V WS2812B strips per meter
- **Less common**: 5V strips more readily available for non-holiday use

**Alternatives Considered**:
1. **5V WS2812B**: Better granularity but more power injection complexity
2. **12V WS2815**: Individual LED control at 12V, but significantly more expensive
3. **SK6812 RGBW**: Includes dedicated white LED, but adds complexity and cost

**Implementation Notes**:
- Use Ray Wu 12V WS2811 bullet pixels with 3" (10cm) spacing
- Plan for IP67 minimum rating for outdoor use
- Order with Ray Wu 3-pin waterproof pigtail connectors for consistency

**Next Steps**:
- Determine total pixel count needed for initial zones
- Calculate exact power requirements
- Add to shopping list with specific supplier links

---

### Entry 3: 2025-12-24 - Home Assistant Integration Approach
**Decision**: Use FPP Home Assistant MQTT Discovery plugin for daily control

**Rationale**:
- **Native integration**: LED zones appear as standard light entities in Home Assistant
- **Familiar interface**: Control matches other smart lights (on/off, color, brightness)
- **Automation friendly**: Easy to create schedules, scenes, and automations
- **Two-way communication**: Status updates flow back to HA for accurate state
- **Community supported**: Plugin is actively maintained and documented

**Trade-offs Accepted**:
- **Setup complexity**: Requires MQTT broker configuration (Mosquitto)
- **Additional layer**: MQTT adds a component between HA and FPP
- **Learning curve**: Understanding overlay models and FPP concepts

**Alternatives Considered**:
1. **Direct HTTP API calls**: More complex automation, no entity discovery
2. **Custom integration**: Requires development effort, maintenance burden
3. **Separate control systems**: Loses smart home integration benefits

**Implementation Notes**:
- Install Mosquitto MQTT broker on Home Assistant host
- Configure FPP overlay models for each logical zone (Front Roof, Side Eaves, etc.)
- Enable FPP HA MQTT plugin and configure MQTT broker connection
- Test discovery and control before physical installation

**Next Steps**:
- Document MQTT setup in software_setup.md
- Reference official FPP plugin documentation
- Plan overlay model structure matching physical zones

---

### Entry 4: 2025-12-24 - Initial Pixel Count and Density
**Decision**: Start with 500 pixels using 12V WS2811 bullet pixels at 10cm (4") spacing

**Rationale**:
- **Manageable scope**: 500 pixels is large enough to create impressive displays but small enough for Phase 1 learning
- **Power supply sizing**: 500 pixels fits well within Mean Well LRS-350-12 capacity (350W) with appropriate headroom
- **Falcon PiCap capacity**: Well within single output capacity (800px max), leaves room for expansion on remaining outputs
- **Cost effective**: ~$125-200 for pixels at bulk pricing, reasonable initial investment
- **Spacing balance**: 10cm spacing provides good visual effect for rooflines while keeping pixel count and cost manageable
- **Future flexibility**: Can add more pixels and vary density in future phases

**Trade-offs Accepted**:
- Not covering entire house in Phase 1 (expansion planned for later phases)
- 10cm spacing may appear slightly sparse for close-up viewing (acceptable for roofline distances)
- Single density choice means all zones will have similar pixel spacing initially

**Alternatives Considered**:
1. **300 pixels**: Too small for impactful display, would likely need immediate expansion
2. **800+ pixels**: Exceeds comfortable Phase 1 scope, higher risk if design changes needed
3. **7.5cm spacing**: Denser effect but 33% more pixels/cost, diminishing returns at viewing distance
4. **Mix of densities**: Added complexity for Phase 1, better suited for future expansion

**Implementation Notes**:
- Order 500x 12V WS2811 bullet pixels, 10cm spacing, IP68 rated
- Plan for 2 zones to utilize 2 of the 4 PiCap outputs
- Exact zone allocation TBD during physical site measurement
- Both-ends power injection strategy for 12V runs

**Next Steps**:
- Measure house rooflines to determine optimal zone breakdown
- Finalize shopping list quantities for 500-pixel installation
- Plan power distribution for 2-zone setup

---

### Entry 5: 2025-12-24 - Controller Location Strategy
**Decision**: Install controller under cover at edge of house with wired Ethernet connection to home network switch

**Rationale**:
- **Weather protection**: Under-cover location protects Pi and PiCap from direct rain/sun exposure while maintaining outdoor proximity
- **Ethernet reliability**: Wired connection eliminates Wi-Fi reliability concerns for permanent outdoor installation
- **Accessibility**: Edge of house location provides reasonable access for troubleshooting and maintenance
- **Cable management**: Proximity to LED start points minimizes data cable runs and signal degradation
- **Network integration**: Direct connection to home switch ensures stable Home Assistant communication
- **Expansion ready**: Location chosen with consideration for future zone additions

**Trade-offs Accepted**:
- Requires running Ethernet cable from indoor network switch to outdoor location
- May need outdoor-rated enclosure even under cover (for humidity/temperature)
- Access requires going outside (vs indoor controller location)
- Specific mounting location TBD during installation phase

**Alternatives Considered**:
1. **Fully indoor controller**: Cleaner environment but would require longer outdoor cable runs for LEDs and potential signal issues
2. **Fully outdoor exposed**: Requires more robust weatherproofing, higher risk of moisture/temperature issues
3. **Garage location**: May be too far from optimal LED zone starting points
4. **Wi-Fi connection**: Rejected due to reliability concerns for permanent installation

**Implementation Notes**:
- Measure Ethernet cable run from indoor switch to proposed outdoor location
- Select IP65+ rated enclosure for controller and PSU
- Plan cable entry points with weatherproof grommets/glands
- PSU location to be determined (likely near controller or may be indoor with power cable run)
- Final placement will be confirmed during installation planning with actual measurements

**Next Steps**:
- Measure Ethernet run length and plan cable routing
- Select appropriate enclosure size for Pi+PiCap+connections
- Determine PSU location (co-located with controller vs separate indoor location)
- Add Ethernet cable and enclosure to shopping list

---

## High-Level Requirements

### Functional Requirements
1. **Daily Lighting Control**
   - Turn lights on/off via Home Assistant
   - Set solid colors (red, green, blue, white, custom)
   - Adjust brightness (0-100%)
   - Control individual zones independently
   - Schedule automated on/off times (sunset/sunrise)

2. **Holiday Show Capability**
   - Play pre-programmed sequences from FPP
   - Synchronize lighting to music
   - Schedule automated show times
   - Quick-change between holiday themes

3. **Home Assistant Integration**
   - Lights appear as native HA entities
   - Respond to HA automations
   - Support voice control via HA integrations
   - Status feedback (on/off state, current color)

4. **Reliability & Safety**
   - Weatherproof outdoor installation (IP67+ rated)
   - Fused outputs for overcurrent protection
   - Proper power injection to prevent voltage drop
   - Stable wired network connectivity

### Non-Functional Requirements
1. **Scalability**: System should support expansion to 3000+ pixels without architecture changes
2. **Maintainability**: Components should be readily available and replaceable
3. **Performance**: Lighting effects should be smooth (>30 FPS) without flickering
4. **Energy Efficiency**: Power consumption optimized, support for dimming
5. **Aesthetics**: Clean installation with hidden wiring and professional appearance

### Constraints
1. **Budget**: Initial phase target ~$500-800 (hardware only, excluding LEDs)
2. **Timeline**: Basic installation complete within 8-10 weeks
3. **Skills**: DIY-friendly, no specialized electronics or programming skills required
4. **Location**: Outdoor installation, must withstand Australian climate
5. **Network**: Wired Ethernet available to controller location

---

## Scope Boundaries

### In Scope
- 2-3 LED zones for initial installation (rooflines, eaves)
- Falcon PiCap + Raspberry Pi controller setup
- Basic FPP configuration and output setup
- Home Assistant MQTT integration
- Essential xLights learning for simple sequences
- Weatherproof mounting for controller and LEDs
- Power supply and distribution for current scope
- Documentation of all processes and configurations

### Out of Scope (for initial phases)
- Complex xLights sequences beyond basic effects (can be added later)
- Integration with external show data sources (cloud sequences, etc.)
- Multi-controller synchronization (future expansion)
- DMX fixtures or other non-LED lighting
- Professional-grade show production
- Community sharing/contribution (until system proven)
- Year-round holiday calendar sequencing (start with Christmas only)
- Integration with music visualization tools beyond basic sync

### Future Considerations
- Expansion to garage, garden, or pathway lighting
- Additional Falcon controllers for distributed control
- Integration with security systems (motion-triggered lighting)
- Weather-reactive displays (temperature, conditions)
- Community sequence library integration
- Participation in local light competitions

---

## References & Resources

### Documentation Structure
- **master_plan.md** (this file): Strategic overview, phases, decisions, architecture
- **docs/current_state/getting_started.md**: Newcomer-friendly quick start guide
- **docs/current_state/hardware_setup.md**: Detailed hardware assembly and configuration
- **docs/current_state/software_setup.md**: FPP, xLights, and integration setup guides
- **docs/current_state/shopping_list.md**: Complete parts list with comparisons and links
- **docs/current_state/decision_checklist.md**: Critical decisions with guidance
- **docs/current_state/home_assistant_integration.md**: HA configuration details
- **.github/copilot-instructions.md**: Repository conventions and coding guidelines

### External Resources
- **Home Assistant Config Repository**: https://github.com/richardthorek/haconfiguration
- **Initial Research Document**: `./initial_research` (technical foundation)
- **Falcon Player Documentation**: https://falconchristmas.github.io/FPP/
- **xLights Documentation**: https://xlights.org/
- **Falcon Hardware**: https://pixelcontroller.com/
- **Community Forums**: 
  - AusChristmasLighting: https://auschristmaslighting.com/
  - FalconChristmas Forum: https://falconchristmas.com/forum/

---

## Success Criteria

### Phase 1 (Planning) - Completed When:
- ✅ master_plan.md created and comprehensive
- ✅ All docs/current_state/ guides created and actionable
- ✅ Shopping list complete with supplier links and comparisons
- ✅ Decision checklist addresses all critical choices
- ✅ Architecture diagrams prompts ready for generation
- ✅ No ambiguities or missing information for Phase 2 start

### Overall Project Success:
- Lights controllable from Home Assistant dashboard
- Basic on/off, color, brightness control functional
- At least one holiday sequence playable via FPP
- Installation weatherproof and aesthetically pleasing
- System runs reliably for one full holiday season
- Documentation sufficient for others to replicate
- User satisfaction: "Happy with the investment and would recommend"

---

## Version History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-12-24 | GitHub Copilot | Initial master plan creation with comprehensive documentation structure, architecture definition, phase breakdown, and decision logging |
| 1.1 | 2025-12-24 | GitHub Copilot | Added Decision Log entries 4 & 5: 500-pixel starting scope with 10cm spacing, controller location strategy under cover with wired Ethernet |

---

*This master plan is a living document and will be updated as the project progresses through each phase. All changes should be logged in the Version History section above.*
