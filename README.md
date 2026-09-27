# Home Christmas Lighting Project

A comprehensive implementation guide for a scalable, year-round addressable LED lighting system using Falcon PiCap, Raspberry Pi, and Home Assistant integration.

## 🎄 Project Overview

This repository documents the complete setup of a home Christmas/holiday lighting system featuring:

- **Hardware**: Raspberry Pi 4 with Falcon PiCap controller
- **LEDs**: 12V WS2811 addressable pixels (scalable to 3000+ pixels)
- **Software**: Falcon Player (FPP) for show management, xLights for sequencing
- **Integration**: Home Assistant via MQTT for smart home control
- **Use Cases**: Daily ambient lighting, holiday displays, synchronized music shows

## 🚀 Quick Start

New to the project? Start here:

1. **[Pi Controller Bring-Up Guide](docs/pi-setup-guide.html)** - Step-by-step Pi 4 + PiCap + FPP setup, wiring diagrams, power budget and project review (open in a browser; supersedes conflicting power advice in the older docs)
2. **[Getting Started Guide](docs/current_state/getting_started.md)** - 5-minute overview and quick start
3. **[Master Plan](master_plan.md)** - Strategic roadmap and architecture overview
4. **[Shopping List](docs/current_state/shopping_list.md)** - Complete parts list with suppliers
5. **[Decision Checklist](docs/current_state/decision_checklist.md)** - Critical decisions to make

**Setting up a laptop to work on this repo?** See [`.devcontainer/README.md`](.devcontainer/README.md) — a devcontainer with git/gh, Python, MQTT tooling and Claude Code preinstalled.

## 📚 Documentation

### Strategic Documents
- **[master_plan.md](master_plan.md)** - Single source of truth for project scope, phases, and decisions
- **[initial_research](docs/archive/initial_research.md)** - Technical research baseline (read-only reference)

### Implementation Guides
- **[Getting Started](docs/current_state/getting_started.md)** - Newcomer-friendly introduction
- **[Hardware Setup](docs/current_state/hardware_setup.md)** - Physical assembly and wiring
- **[Software Setup](docs/current_state/software_setup.md)** - FPP, xLights, and MQTT configuration
- **[Shopping List](docs/current_state/shopping_list.md)** - Complete parts list with pricing
- **[Decision Checklist](docs/current_state/decision_checklist.md)** - Critical decisions with pros/cons
- **[Home Assistant Integration](docs/current_state/home_assistant_integration.md)** - HA configuration details

## 🏗️ System Architecture

```
[Home Assistant] ←MQTT→ [Raspberry Pi + Falcon PiCap] → [12V WS2811 LED Pixels]
     (Control)                  (Controller)                    (Lights)
```

**Key Components**:
- Raspberry Pi 4 (4GB) running Falcon Player (FPP)
- Falcon PiCap HAT (2-4 outputs, 800 pixels each)
- 12V WS2811 addressable LED pixels (300-1500+ pixels)
- Mean Well LRS-350-12 power supply (350W)
- Home Assistant with MQTT broker integration

## 📦 Current Status

**Phase**: Planning & Documentation (as of 2025-12-24)

**Current Configuration** (Decisions finalized 2025-12-24):
- ✅ Controller: Falcon PiCap + Raspberry Pi 4 selected
- ✅ LEDs: 500× 12V WS2811 bullet pixels, 10cm spacing
- ✅ Power: Mean Well LRS-350-12 (350W)
- ✅ Network: Wired Ethernet
- ✅ Integration: FPP MQTT Plugin for Home Assistant
- ✅ Installation: 2 zones, controller under cover at edge of house

**Documentation Progress**:
- ✅ Master plan created with decision log entries
- ✅ Complete documentation structure established
- ✅ Shopping list with supplier links and chosen configuration highlighted
- ✅ Decision checklist completed with specific choices
- ✅ Architecture diagrams prompts prepared
- ⏳ Hardware procurement (next phase)

See [master_plan.md](master_plan.md) for detailed phase breakdown and timeline.

## 🛠️ Technology Stack

- **Controller**: Raspberry Pi 4 + Falcon PiCap
- **Software**: Falcon Player (FPP) v7.x+
- **Sequencing**: xLights
- **Integration**: Home Assistant + MQTT (Mosquitto)
- **LEDs**: 12V WS2811 addressable pixels
- **Network**: Wired Ethernet

## 🎯 Project Goals

1. **Daily Use**: Simple color control via Home Assistant for year-round lighting
2. **Holiday Shows**: Synchronized music and animation sequences for Christmas, Halloween, etc.
3. **Smart Home**: Full integration with Home Assistant automations and voice control
4. **Scalability**: Foundation to expand from 500 to 3000+ pixels without architecture changes
5. **Reliability**: Wired, weatherproof outdoor installation with minimal maintenance

## 📖 Related Resources

- **Home Assistant Config**: [richardthorek/haconfiguration](https://github.com/richardthorek/haconfiguration)
- **FPP Documentation**: https://falconchristmas.github.io/FPP/
- **xLights**: https://xlights.org/
- **Falcon Hardware**: https://pixelcontroller.com/
- **Community**: [AusChristmasLighting Forums](https://auschristmaslighting.com/)

## 🤝 Contributing

This is a personal project repository, but suggestions and improvements are welcome! 

- Open an issue for documentation corrections or suggestions
- Share your own implementation experiences in discussions
- Link back to this repo if you create derivative guides

## 📄 License

Documentation: CC BY-SA 4.0 (Creative Commons Attribution-ShareAlike)

## 🙏 Acknowledgments

- Falcon Player (FPP) open-source project
- xLights sequencing software community
- Home Assistant community
- AusChristmasLighting forum members
- Initial research and guidance from holiday lighting enthusiasts worldwide

---

**Ready to start?** Head to the [Getting Started Guide](docs/current_state/getting_started.md)!