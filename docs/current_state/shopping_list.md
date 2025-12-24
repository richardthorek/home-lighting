# Complete Shopping List - Home LED Lighting Project

## Overview

This shopping list provides all hardware and materials needed for the Falcon PiCap + Raspberry Pi lighting system. Items are organized by category with supplier links, pricing (as of 2025-12), and quantity recommendations.

**Budget Summary**:
- **Minimum Test Setup**: ~$175-215 USD (50 pixels for testing)
- **Phase 1 Full Setup** (500 pixels, 2 zones): ~$500-700 USD
- **Phase 2 Expansion** (1500 pixels, 4-5 zones): ~$900-1200 USD

---

## Core Controller Components

### Raspberry Pi & Accessories

| Item | Specs | Qty | Unit Price | Total | Supplier | Notes |
|------|-------|-----|------------|-------|----------|-------|
| **Raspberry Pi 4 Model B** | 4GB RAM | 1 | $55 USD | $55 | [Adafruit](https://www.adafruit.com/product/4296), [PiAustralia](https://raspberry.piaustralia.com.au/) | 2GB version works, but 4GB recommended for future expansion |
| **Falcon PiCap v2** | 2-4 outputs, fused | 1 | $50 USD | $50 | [PixelController](https://pixelcontroller.com/store/index.php?id_product=56&controller=product), [Hanson Electronics AU](https://www.hansonelectronics.com.au/) | Core controller board - this is the main purchase |
| **MicroSD Card** | 32GB, Class 10, A1 rated | 1 | $10 USD | $10 | Amazon, local electronics | For FPP OS, 32GB is plenty |
| **Pi Power Supply** | 5V 3A USB-C (official) | 1 | $8-12 USD | $10 | [Raspberry Pi Store](https://www.raspberrypi.com/products/type-c-power-supply/) | Only if not using PiCap power pass-through |
| **Ethernet Cable** | Cat5e/6, length as needed | 1 | $5-15 USD | $10 | Local hardware store | Measure distance from router to controller |
| **Raspberry Pi Case** | With fan, compatible with HAT | 1 | $10-20 USD | $15 | Amazon, AliExpress | Optional but recommended - look for cases supporting HATs |

**Subtotal**: ~$150 USD

**Alternative for Australia**: 
- **Hanson rPi-28D+**: $38 AUD (~$25 USD) - Alternative to Falcon PiCap with 2 outputs, FPP compatible [Hanson Electronics](https://www.hansonelectronics.com.au/product/rpi-28d/)

---

## LED Pixels & Strings

### 12V WS2811 Addressable LEDs (Recommended)

| Type | Specs | Qty | Unit Price | Total | Supplier | Use Case |
|------|-------|-----|------------|-------|----------|----------|
| **Ray Wu 12V Bullet Pixels** | WS2811, 3-LED, 10cm spacing, IP68 | 50 (test) | $0.30-0.50 ea | $15-25 | [Ray Wu Store (AliExpress)](https://www.aliexpress.com/store/1047122) | Initial testing |
| **Ray Wu 12V Bullet Pixels** | WS2811, 3-LED, 10cm spacing, IP68 | 500 (Phase 1) | $0.25-0.40 ea | $125-200 | [Ray Wu Store](https://www.aliexpress.com/store/1047122) | Full roofline install |
| **Ray Wu 12V Bullet Pixels** | WS2811, 3-LED, 10cm spacing, IP68 | 1000+ (expansion) | $0.22-0.35 ea | $220-350 | [Ray Wu Store](https://www.aliexpress.com/store/1047122) | Large displays |
| **12V WS2811 LED Strip** | 60 LED/m (20 pixels/m), IP67, 5m reel | 1-2 reels | $25-40/reel | $50-80 | Amazon, AliExpress | Continuous light lines (alternative to bullets) |

**Notes**:
- **Spacing Options**: 3" (7.5cm), 4" (10cm), 6" (15cm) - Choose based on desired density
- **Connector Type**: Order with **Ray Wu 3-pin JST-SM waterproof connectors** pre-attached
- **Colors**: Order "RGB" (full color) not single-color strings
- **IP Rating**: IP67 minimum for outdoor, IP68 for harsh weather areas

### Alternative LED Options (for comparison)

| Type | Voltage | Pros | Cons | Price Range |
|------|---------|------|------|-------------|
| **12V WS2811 Bullets** ⭐ | 12V | Best for long runs, less power injection, outdoor proven | Groups of 3 LEDs (less granular) | $0.25-0.40/pixel |
| **5V WS2812B Strip** | 5V | Individual LED control, widely available, cheaper | More power injection needed, 5m max runs | $0.15-0.30/pixel |
| **12V WS2815 Strip** | 12V | Individual LEDs + 12V benefits, backup data line | More expensive, harder to find | $0.45-0.70/pixel |
| **12V WS2811 Strip** | 12V | Cheaper than bullets, continuous line | Groups of 3, harder to repair individual LEDs | $0.20-0.35/pixel |

**Recommendation**: **12V WS2811 Bullet Pixels** for rooflines/outlines, **12V WS2815 Strip** for fill areas if budget allows.

---

## Power Supplies

### Primary Power Supply

| Item | Specs | Qty | Unit Price | Total | Supplier | Notes |
|------|-------|-----|------------|-------|----------|-------|
| **Mean Well LRS-350-12** | 12V 29A 350W, enclosed | 1 | $35-50 USD | $40 | [Digi-Key](https://www.digikey.com/), [Mouser](https://www.mouser.com/), Amazon | High quality, UL listed, recommended |
| **Generic 12V 30A PSU** | 12V 30A 360W, enclosed | 1 | $25-35 USD | $30 | Amazon, eBay | Budget option, check reviews |
| **Mean Well LRS-100-12** | 12V 8.5A 100W | 1 | $15-25 USD | $20 | Digi-Key, Mouser | For small test setups only |

**Power Calculation Guide**:
```
Pixels × 0.05A × 12V = Max Wattage
Example: 500 pixels × 0.05A × 12V = 300W (use 350W supply)

Rule of thumb: 
- 50-100 pixels: 100W supply
- 300-500 pixels: 350W supply  
- 500-1000 pixels: 600W supply
- 1000-2000 pixels: 2× 350W or 1× 1000W supply
```

**Notes**:
- Always add 20-30% headroom to calculations
- Use enclosed supplies (safer, weatherproof-ready)
- Mean Well is industry standard for reliability
- For large installs (1500+ pixels), consider multiple smaller supplies

### Power Distribution & Safety

| Item | Specs | Qty | Unit Price | Total | Supplier |
|------|-------|-----|------------|-------|----------|
| **Power Distribution Block** | 12V, 10-way screw terminals | 1-2 | $8-15 | $10 | Amazon, eBay |
| **Inline Blade Fuses** | 5A, 7.5A, 10A assortment | 1 pack | $10-15 | $12 | Auto parts store, Amazon |
| **Inline Fuse Holders** | Blade type, 18AWG | 5-10 | $1-2 ea | $10 | Amazon, auto parts |
| **Circuit Breaker** | 15A, 12V DC rated | 1 | $8-12 | $10 | Amazon, marine supply |
| **GFCI Outlet** (AC side) | 120V/240V, outdoor rated | 1 | $15-25 | $20 | Home Depot, Bunnings |

**Subtotal (Power)**: ~$90-120 USD

---

## Wiring & Connectors

### Wire & Cable

| Item | Gauge | Length | Unit Price | Total | Supplier | Use |
|------|-------|--------|------------|-------|----------|-----|
| **18AWG 3-conductor cable** | 18AWG, 3-core, outdoor rated | 30m (100ft) | $25-40 | $35 | Amazon, electrical supply | Main data + power runs |
| **18AWG 2-conductor cable** | 18AWG, 2-core, red/black | 30m (100ft) | $20-30 | $25 | Amazon, electrical supply | Power injection lines |
| **16AWG 2-conductor cable** | 16AWG (heavier), red/black | 15m (50ft) | $20-30 | $25 | Amazon, electrical supply | Long power runs, high current |
| **22AWG 3-conductor cable** | 22AWG, 3-core | 10m (30ft) | $10-15 | $12 | Amazon | Pigtail extensions, short runs |

**Notes**:
- Lower AWG = thicker wire = less voltage drop
- Use 18AWG for runs under 5m, 16AWG for 5-10m, 14AWG for 10m+
- Outdoor-rated jacket (UV resistant) for exposed wiring
- Color coding: Red = +12V, Black/Blue = Ground, Green/White = Data

### Connectors & Hardware

| Item | Specs | Qty | Unit Price | Total | Supplier |
|------|-------|-----|------------|-------|----------|
| **Ray Wu 3-pin JST-SM Connectors** | Male/female pairs, waterproof | 20 pairs | $0.40-0.60 ea | $10 | [Ray Wu Store](https://www.aliexpress.com/store/1047122), Amazon |
| **Dupont Connectors** | 3-pin, 2.54mm pitch | 10 sets | $0.20 ea | $2 | Amazon, AliExpress |
| **Screw Terminal Blocks** | 3-position, 5mm pitch | 10 | $0.50 ea | $5 | Amazon, electrical supply |
| **Heat Shrink Tubing** | Assortment, 3:1 ratio, adhesive | 1 kit | $10-15 | $12 | Amazon |
| **Silicone Sealant** | Clear or black, outdoor rated | 1 tube | $5-8 | $6 | Hardware store |
| **Dielectric Grease** | Corrosion prevention | 1 tube | $8-12 | $10 | Auto parts, Amazon |
| **Cable Ties (UV resistant)** | Black, assorted sizes | 1 pack (100) | $8-12 | $10 | Hardware store |
| **Wire Nuts** | Waterproof, assortment | 1 pack | $8-12 | $10 | Hardware store |

**Subtotal (Wiring)**: ~$160 USD

---

## Mounting Hardware

### For Bullet Pixels (Roofline/Outline)

| Item | Description | Qty | Unit Price | Total | Supplier |
|------|-------------|-----|------------|-------|----------|
| **Boscoyo Mounting Strips** | Plastic strip, pre-drilled holes | 10m (33ft) | $2-3/ft | $60-90 | [Boscoyo Studio](https://boscoyostudio.com/) |
| **Pixel Mounting Clips** | Plastic clips for 12mm bullets | 500 | $0.05-0.10 ea | $25-50 | AliExpress, Amazon |
| **Gutter Hooks** | Plastic, for hanging from gutters | 50 | $0.20 ea | $10 | Hardware store |
| **Command Outdoor Strips** | 3M adhesive, removable | 2 packs | $10/pack | $20 | Home Depot, Bunnings |

### For LED Strips (Continuous Lines)

| Item | Description | Qty | Unit Price | Total | Supplier |
|------|-------------|-----|------------|-------|----------|
| **Aluminum LED Channel** | U-channel, 1m segments, diffuser | 10m | $5-10/m | $60-100 | Amazon, AliExpress |
| **Mounting Clips for Channels** | Metal clips for U-channel | 50 | $0.20 ea | $10 | Amazon |
| **Silicone LED Clips** | Clear clips for direct strip mounting | 100 | $0.10 ea | $10 | Amazon |

### Enclosures

| Item | Specs | Qty | Unit Price | Total | Supplier | Use |
|------|-------|-----|------------|-------|----------|-----|
| **Controller Enclosure** | IP65, 200×120×75mm, clear lid | 1 | $15-25 | $20 | Amazon, electrical supply | Houses Pi + PiCap |
| **Power Supply Enclosure** | IP65, 300×200×150mm | 1 | $25-40 | $30 | Amazon, electrical supply | Houses 350W PSU |
| **Junction Boxes** | IP67, 100×100×50mm | 3-5 | $8-12 ea | $30 | Hardware store | Wire splicing, injections |
| **Cable Glands** | PG7, PG9 sizes, waterproof | 10 | $0.50-1 ea | $7 | Amazon, electrical supply | Cable entry to enclosures |
| **Mounting Brackets** | Adjustable, stainless steel | 2 sets | $5-10/set | $12 | Hardware store | Secure enclosures |

**Subtotal (Mounting)**: ~$175-270 USD (varies by LED type chosen)

---

## Tools & Supplies

### Essential Tools (if you don't have)

| Item | Description | Price Range | Supplier |
|------|-------------|-------------|----------|
| **Wire Strippers** | 10-22AWG range | $10-25 | Hardware store |
| **Crimping Tool** | Dupont, JST connectors | $15-30 | Amazon |
| **Multimeter** | Voltage, continuity testing | $15-40 | Harbor Freight, Amazon |
| **Soldering Iron Kit** | 60W, with solder, flux | $20-50 | Amazon |
| **Heat Gun** | For heat shrink tubing | $15-35 | Harbor Freight, Amazon |
| **Drill + Bits** | For mounting holes | $30-100 | Hardware store |
| **Cable Tester** | RJ45/Cat5 tester (optional) | $10-20 | Amazon |

### Consumables

| Item | Qty | Price | Notes |
|------|-----|-------|-------|
| **Solder** | 1 roll | $8-12 | 60/40 or lead-free |
| **Flux Paste** | 1 tube | $5-8 | For better solder joints |
| **Electrical Tape** | 2 rolls | $5 | Quality vinyl tape |
| **Zip Ties** | 1 pack | $8-12 | UV-resistant black |
| **Label Maker** or **Labels** | 1 | $10-30 | For wire identification |

---

## Optional / Nice-to-Have

| Item | Purpose | Price | Priority |
|------|---------|-------|----------|
| **Raspberry Pi PoE+ HAT** | Power Pi via Ethernet | $20-30 | Low (simplifies wiring but adds cost) |
| **Weatherproof Ethernet Coupler** | RJ45 IP67-rated | $10-15 | Medium (if cable joins outdoors) |
| **C7/C9 Bulb Covers** | Aesthetic covers for pixel nodes | $15-25/50 | Low (cosmetic only) |
| **RGB Flood Lights** | Accent lighting for house/trees | $30-60 ea | Low (expansion) |
| **Extension Cords** (outdoor) | AC power to controller location | $15-30 | High (if no outlet nearby) |
| **Surge Protector** | Protect controller/PSU | $15-25 | Medium (good practice) |
| **UPS Battery Backup** | Keep Pi running in power blips | $60-100 | Low (nice for reliability) |

---

## Recommended Bundles by Phase

### Test Setup Bundle (~$200)
- Raspberry Pi 4 (4GB)
- Falcon PiCap v2
- MicroSD card (32GB)
- Mean Well LRS-100-12 (100W PSU)
- 50× 12V WS2811 pixels with connectors
- 10m 18AWG 3-conductor cable
- 10× Ray Wu connectors
- Heat shrink kit
- Ethernet cable

**Purpose**: Test everything on a bench, learn FPP and Home Assistant integration

---

### Phase 1 Installation Bundle (~$650)
Everything from Test Setup, plus:
- Mean Well LRS-350-12 (350W PSU) – upgrade
- 500× 12V WS2811 pixels
- 30m 18AWG 3-conductor cable
- 30m 16AWG 2-conductor cable
- 20× Ray Wu connectors
- Controller + PSU enclosures (IP65)
- 3× junction boxes
- Mounting clips or Boscoyo strips (10m)
- Fuses and distribution block

**Purpose**: Full roofline or 2-zone installation

---

### Phase 2 Expansion Bundle (~$400 additional)
- 1000× additional 12V WS2811 pixels
- Second 350W PSU or upgrade to 600W
- Additional mounting hardware
- More wiring and connectors as needed

**Purpose**: Expand to 3-5 zones, larger display

---

## Supplier Reference Links

### Primary Suppliers

**North America**:
- [Adafruit](https://www.adafruit.com/) - Raspberry Pi, accessories
- [PixelController.com](https://pixelcontroller.com/store/) - Falcon hardware
- [Amazon](https://www.amazon.com/) - General supplies, wire, tools
- [Digi-Key](https://www.digikey.com/) - Mean Well PSUs, electronics
- [Boscoyo Studio](https://boscoyostudio.com/) - Mounting supplies

**Australia**:
- [Hanson Electronics](https://www.hansonelectronics.com.au/) - Falcon alternatives, controllers
- [Core Electronics](https://core-electronics.com.au/) - Raspberry Pi, accessories
- [Bunnings](https://www.bunnings.com.au/) - Mounting hardware, tools
- [Jaycar](https://www.jaycar.com.au/) - Electronics, wire, connectors

**International (LED Pixels)**:
- [Ray Wu Store (AliExpress)](https://www.aliexpress.com/store/1047122) - Best prices on WS2811 pixels
- [BTF-Lighting (AliExpress)](https://www.aliexpress.com/store/105645) - LED strips, pixels

### Specialty Suppliers
- [Wally's Lights](https://www.wallyslights.com/) - Holiday lighting specialty (US)
- [HolidayCoro](https://www.holidaycoro.com/) - Controllers, pixels (US)
- [Christmas Light Emporium](https://www.christmaslightemporium.com/) - Wide selection (US)

---

## Pricing Notes & Tips

1. **Order in Bulk**: Pixels drop in price at 100+, 500+, 1000+ quantities
2. **Watch for Sales**: AliExpress has big sales in November (Singles Day) and holidays
3. **Shipping Time**: AliExpress can be 2-6 weeks – order early!
4. **Customs/Import**: Consider duties/taxes for international orders
5. **Local First**: Check local electronics stores – often competitive on wire/connectors
6. **Quality vs Price**: For PSUs and controllers, favor quality brands (Mean Well, Falcon)
7. **Test Before Buying Bulk**: Order 50 pixels first, test, then order 1000s

---

## Updated: 2025-12-24

Prices and availability subject to change. Always verify current pricing and stock before ordering.
