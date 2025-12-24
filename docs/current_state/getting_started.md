# Getting Started - Home LED Lighting Project

## Welcome! 👋

This guide will help you understand the home LED lighting project and get started with implementation. Whether you're new to addressable LEDs or an experienced DIYer, this guide provides the essential information you need.

## What Is This Project?

This project creates a **year-round addressable LED lighting system** for your home that can:
- Display simple colors for daily ambient lighting
- Run complex synchronized light shows for holidays
- Integrate with Home Assistant for smart home control
- Scale from a few hundred to thousands of LEDs

**Think of it as**: Permanent Christmas lights that work all year and can be controlled from your phone!

---

## Quick Start: 5 Steps to Success

### 1. Understand the System Architecture (5 minutes)

Our system has four main parts:

```
[Home Assistant] ←→ [Raspberry Pi + Falcon PiCap] ←→ [LED Pixels] ←→ [Power Supply]
     (Control)           (Controller)                (Lights)        (Power)
```

- **Home Assistant**: Your smart home dashboard (controls everything)
- **Raspberry Pi + Falcon PiCap**: The "brain" that tells LEDs what to do
- **LED Pixels**: Individual controllable lights (WS2811 type)
- **Power Supply**: 12V DC power for the LEDs and controller

**Data Flow**: You tap "turn on red lights" in Home Assistant → message goes to Raspberry Pi → Pi tells Falcon PiCap → PiCap sends signals to LEDs → LEDs light up red!

### 2. Check Prerequisites (10 minutes)

Before ordering parts, verify you have:

- ✅ **Home Assistant** already running (or willingness to set it up)
- ✅ **Ethernet connection** available where controller will be located
- ✅ **Outdoor power outlet** or ability to run power cable
- ✅ **Basic tools**: Screwdriver, drill, wire strippers, multimeter (recommended)
- ✅ **Ladder or lift** for roofline installation (if applicable)
- ✅ **Budget**: ~$500-800 for initial setup (excluding LEDs)

**Don't have Home Assistant?** That's okay! Set it up first using their [Getting Started Guide](https://www.home-assistant.io/getting-started/).

### 3. Review the Shopping List (15 minutes)

See **[docs/current_state/shopping_list.md](./shopping_list.md)** for the complete parts list.

**Minimum viable order** (to get started with a test setup):
- Raspberry Pi 4 (4GB): ~$55 USD
- Falcon PiCap v2: ~$50 USD  
- MicroSD card (32GB): ~$10 USD
- 12V Power Supply (100W for testing): ~$20 USD
- 50 x 12V WS2811 pixels (test string): ~$15-25 USD
- Ray Wu 3-pin connectors (10 pack): ~$5 USD
- 18AWG wire (10m): ~$10 USD

**Total for test setup**: ~$175-215 USD

**Phase 1 Full Installation** (current target configuration):
- All test setup components above
- Upgrade to Mean Well LRS-350-12 (350W): ~$40 USD
- 500× 12V WS2811 bullet pixels (10cm spacing): ~$125-200 USD
- Additional wiring and connectors: ~$60 USD
- Enclosures and mounting hardware: ~$80-120 USD
- Fuses, distribution, safety gear: ~$40 USD

**Total for Phase 1**: ~$500-700 USD ⭐

### 4. Make Critical Decisions (20 minutes)

Work through the **[Decision Checklist](./decision_checklist.md)** to answer questions like:
- Where will you mount the LEDs? (Roofline, eaves, pathways?)
- How many pixels do you need?
- Indoor or outdoor controller enclosure?
- Will you create custom sequences, or use pre-made ones?

**Pro tip**: Start small! Order enough for 1-2 zones first, test everything, then expand.

**Current Status**: ✅ Key decisions made (see Configuration section above). Remaining decisions (mounting method, specific zone layout) will be determined during installation planning.

### 5. Follow the Implementation Phases

Our master plan breaks the project into manageable phases:

| Phase | Description | Duration | Key Outcome |
|-------|-------------|----------|-------------|
| 1. Planning | Read docs, order parts | 1 week | Parts ordered |
| 2. Test Bench | Assemble Pi+PiCap, test LEDs indoors | 1 week | Working test setup |
| 3. Integration | Connect to Home Assistant | 1 week | Control from HA |
| 4. Installation | Mount LEDs outdoors | 2-3 weeks | Installed lights |
| 5. Sequences | Create holiday shows in xLights | Ongoing | Custom effects |

**Current Phase**: Planning (you are here!)

---

## ✨ Current Project Configuration

Based on decisions made on 2025-12-24, here's the chosen configuration for this build:

**Hardware Selection**:
- **Controller**: Falcon PiCap + Raspberry Pi 4 (4GB)
- **LEDs**: 500× 12V WS2811 Bullet Pixels
- **Spacing**: 10cm (4") between pixels (~50m total coverage)
- **Power**: Mean Well LRS-350-12 (350W supply)
- **Zones**: 2 zones initially (2 of 4 PiCap outputs used)

**Architecture Decisions**:
- **Network**: Wired Ethernet for reliability
- **Location**: Controller under cover at edge of house
- **Power Strategy**: Both-ends injection on 12V runs
- **Integration**: FPP MQTT Plugin → Home Assistant
- **Form Factor**: Bullet pixels for rooflines (strips may be added later)

**Phase 1 Budget Target**: ~$500-700 USD for complete 500-pixel installation

**Why These Choices?**
- **12V WS2811**: Industry standard for outdoor permanent installations, allows longer runs
- **500 pixels**: Sweet spot for impressive display while learning the system
- **10cm spacing**: Good balance of visual effect and cost for roofline viewing distances
- **Falcon PiCap**: Room to expand to 3200 pixels without replacing controller
- **Wired Ethernet**: Most reliable connection for permanent outdoor installation

See **[decision_checklist.md](./decision_checklist.md)** for detailed rationale and alternatives considered.

---

## What You'll Learn

By completing this project, you'll gain skills in:

1. **Electronics**: DC power systems, voltage drop, fusing, power injection
2. **Networking**: Ethernet, MQTT, IP addressing
3. **Linux**: Raspberry Pi setup, SSH, basic command line
4. **Smart Home**: Home Assistant configuration, automations, MQTT
5. **Creative**: xLights sequencing, effect design
6. **Installation**: Mounting, weatherproofing, cable management

---

## Common Questions

### "Is this hard? I'm not very technical."
The project has some complexity, but it's very DIY-friendly. If you can:
- Install Home Assistant (or use Home Assistant OS)
- Follow step-by-step guides
- Troubleshoot with online resources (forums, YouTube)

Then you can do this! The community is very helpful.

### "How long does it take?"
- **Test bench setup**: 1-2 evenings (4-6 hours)
- **Home Assistant integration**: 1 evening (2-4 hours)
- **Physical installation**: 1-3 weekends depending on scale
- **Learning xLights**: Ongoing (start basic, improve over time)

### "What's the hardest part?"
Most people say:
1. **xLights learning curve** (creating sequences is an art!)
2. **Physical installation** (working at height, weatherproofing)
3. **Power calculations** (getting injection points right)

The good news: Excellent tutorials exist for all of these!

### "Can I start smaller?"
**Yes!** Here's a minimal approach:
- Order only 1 test string (50 pixels)
- Set up Pi + PiCap on a desk
- Test with Home Assistant control
- Add 1 roofline zone
- Stop there or expand later

This gets you 80% of the experience at 20% of the cost.

### "What if something doesn't work?"
- **FPP won't start**: Check SD card image, Pi power supply
- **LEDs don't light**: Verify power voltage, data line connection, correct pixel type in FPP
- **Home Assistant can't connect**: Check MQTT broker config, FPP plugin settings
- **Voltage drop/dim LEDs**: Add power injection points

See **[hardware_setup.md](./hardware_setup.md)** and **[software_setup.md](./software_setup.md)** for detailed troubleshooting.

### "Can I use this with Alexa/Google Home?"
Yes! Home Assistant integrates with both. Once LEDs are in HA, you can say "Alexa, turn the Christmas lights red" and it works!

---

## Next Steps

1. **Read the Master Plan**: [master_plan.md](../../master_plan.md) - Get the big picture
2. **Review Hardware Guide**: [hardware_setup.md](./hardware_setup.md) - Understand the components
3. **Check Shopping List**: [shopping_list.md](./shopping_list.md) - Price out the parts
4. **Work Through Decisions**: [decision_checklist.md](./decision_checklist.md) - Make your choices
5. **Order Parts**: Use the shopping list supplier links
6. **While Waiting for Parts**: 
   - Watch xLights tutorials: [xLights YouTube Channel](https://www.youtube.com/@xLights)
   - Read FPP manual: [FPP Docs](https://falconchristmas.github.io/FPP/)
   - Join community: [AusChristmasLighting Forum](https://auschristmaslighting.com/)

---

## Help & Support

### Official Documentation
- **This Project**: All docs in `docs/current_state/`
- **Falcon Player (FPP)**: https://falconchristmas.github.io/FPP/
- **xLights**: https://xlights.org/docs/
- **Home Assistant**: https://www.home-assistant.io/docs/

### Community Resources
- **AusChristmasLighting Forums**: https://auschristmaslighting.com/ (Very active, Australia-focused)
- **FalconChristmas Forums**: https://falconchristmas.com/forum/ (Falcon hardware specific)
- **Home Assistant Community**: https://community.home-assistant.io/
- **Reddit**: r/ChristmasLights, r/homeassistant

### YouTube Channels
- **Canispater (FPP & xLights)**: Excellent beginner tutorials
- **ControllerConnect**: Product reviews and setup guides
- **Dr. Zzs**: Home Assistant integration focus

---

## Tips for Success

1. **Start Small**: Don't order 5,000 pixels on day one
2. **Test First**: Get 50-100 pixels working before installing
3. **Ask Questions**: The community loves helping newcomers
4. **Take Photos**: Document your build for troubleshooting
5. **Plan Wiring**: Sketch it out before running cables
6. **Weatherproof Everything**: Even if it looks sealed, add silicone/heat shrink
7. **Label Wires**: Future you will thank present you
8. **Enjoy the Process**: This is supposed to be fun!

---

## Ready to Go?

Great! Your next stop is the **[Shopping List](./shopping_list.md)** to start ordering parts.

**Questions?** Check the other guides in `docs/current_state/` or reach out to the community!

Happy building! 🎄✨
