# GitHub Copilot Instructions for home-lighting Repository

## Repository Overview

This repository documents the implementation of a home Christmas/holiday LED lighting system using:
- **Hardware**: Raspberry Pi with Falcon PiCap controller
- **LEDs**: 12V WS2811 addressable pixels
- **Software**: Falcon Player (FPP), xLights for sequencing
- **Integration**: Home Assistant via MQTT
- **Purpose**: Year-round addressable lighting with holiday show capability

## Documentation Structure

The repository follows the **Pace-Applied-Solutions** conventions:

```
home-lighting/
├── master_plan.md                    # Single source of truth - strategic overview
├── initial_research                  # Technical research baseline (read-only reference)
├── docs/
│   └── current_state/               # Implementation guides
│       ├── getting_started.md       # Newcomer-friendly quick start
│       ├── hardware_setup.md        # Detailed hardware assembly & wiring
│       ├── software_setup.md        # FPP, xLights, MQTT configuration
│       ├── shopping_list.md         # Complete parts list with suppliers
│       ├── decision_checklist.md    # Critical decision guidance
│       └── home_assistant_integration.md  # HA configuration details
├── .github/
│   └── copilot-instructions.md      # This file - repository conventions
└── README.md                         # High-level repository description
```

## Documentation Conventions

### Master Plan (`master_plan.md`)

**Purpose**: Strategic roadmap and decision log  
**Contents**:
- Executive summary and architecture overview
- Phase breakdown with timelines and deliverables
- Decision log with dated entries (rationale, trade-offs, alternatives)
- Success criteria and scope boundaries
- Version history

**When to Update**:
- ✅ At the start of each new phase
- ✅ When major decisions are made (hardware choice, architecture change)
- ✅ When blockers or risks are identified
- ✅ When phases complete (update status, log lessons learned)

**How to Update**:
- Add new dated entries to Decision Log section
- Update phase status tables
- Increment version number in Version History
- Keep it high-level and strategic (not implementation details)

### Current State Documentation (`docs/current_state/`)

**Purpose**: Tactical implementation guides  
**Contents**:
- Step-by-step instructions for hardware assembly, software installation, integration
- Shopping lists with specific product links and pricing
- Troubleshooting guides
- Configuration examples (YAML, code snippets)
- Screenshots and diagrams where helpful

**When to Update**:
- ✅ When implementation steps change
- ✅ When new troubleshooting solutions are discovered
- ✅ When product links break or pricing changes significantly
- ✅ When users report confusion or missing information

**How to Update**:
- Keep information actionable and specific
- Include exact commands, file paths, configuration snippets
- Link to official external documentation where appropriate
- Add "Updated: YYYY-MM-DD" date at bottom of file

### README.md

**Purpose**: Repository landing page  
**Contents**:
- Brief project description
- Quick links to key documentation
- Status badges (if applicable)
- License and contribution information

**Keep It Brief**: Detailed information belongs in `master_plan.md` or `docs/current_state/`

---

## Code & Configuration Standards

### YAML Files (Home Assistant, FPP, xLights)

**Style**:
- Use 2-space indentation (never tabs)
- Include comments explaining non-obvious configuration
- Group related items with blank lines
- Use meaningful names for entities, automations, scenes

**Example**:
```yaml
# Good
automation:
  - id: christmas_lights_sunset_on
    alias: "Christmas Lights - On at Sunset"
    description: "Turn on lights 30 minutes before sunset"
    trigger:
      - platform: sun
        event: sunset
        offset: "-00:30:00"
    action:
      - service: light.turn_on
        target:
          entity_id: light.all_christmas_lights
        data:
          brightness: 255
          rgb_color: [255, 200, 100]  # Warm white
```

### Markdown Documentation

**Style**:
- Use ATX-style headers (`#`, `##`, `###`)
- Include table of contents for long documents (use `---` dividers)
- Use code blocks with language specifiers (```yaml, ```bash, etc.)
- Use tables for comparisons and lists with multiple attributes
- Use checkboxes for checklists (`- [ ]` or `- [x]`)
- Use emoji sparingly for visual breaks (⭐, ✅, ⏳, ☐)

**Links**:
- Use relative links for internal documentation: `[hardware_setup.md](./hardware_setup.md)`
- Use absolute URLs for external resources: `[FPP Docs](https://falconchristmas.github.io/FPP/)`
- Verify links are not broken when updating

**Examples**:
- Include realistic examples with actual values, not placeholders
- Annotate examples with comments explaining key parts
- Show both "good" and "bad" examples where helpful

---

## Content Guidelines

### Technical Accuracy

- **Verify all technical specifications** (voltage, current, pixel counts, etc.)
- **Link to official documentation** for critical information (don't duplicate what upstream sources maintain)
- **Note versioning** when referencing software (e.g., "FPP v7.x" not just "FPP")
- **Update dates** when prices, availability, or technical details change

### Beginner-Friendly Language

- **Define technical terms** on first use (or link to glossary)
- **Explain "why"** not just "how" (rationale for decisions)
- **Provide context** for recommendations (who is this best for?)
- **Include troubleshooting** for common issues in each guide

### Safety & Best Practices

**Always include safety warnings for**:
- AC mains power work (recommend electrician if unsure)
- Ladder/height safety for installations
- Electrical safety (fusing, polarity, voltage verification)
- Weatherproofing requirements for outdoor equipment

**Example**:
```markdown
**Warning**: AC mains power is dangerous. If you're not comfortable with AC wiring, hire a licensed electrician.
```

---

## Common Tasks & Instructions for Copilot

### Task: Update Hardware Shopping List

**When**: Product links break, prices change significantly, new products available

**Steps**:
1. Verify current pricing on supplier websites
2. Update `docs/current_state/shopping_list.md`
3. Check for better alternatives or new products
4. Update "Updated: YYYY-MM-DD" at bottom
5. If major changes, note in commit message

**Maintain**:
- Multiple supplier options (AU and US/International)
- Price ranges (budget, recommended, premium)
- Comparative tables (pros/cons)

### Task: Add New Automation Example

**When**: User requests or discovers useful automation pattern

**Steps**:
1. Add to `docs/current_state/home_assistant_integration.md` under appropriate section
2. Include complete YAML code block with comments
3. Explain trigger, condition, action logic
4. Note any prerequisites (helper entities, integrations)
5. Test YAML syntax (validate in HA if possible)

### Task: Document New FPP Feature

**When**: FPP updates with new relevant feature

**Steps**:
1. Update `docs/current_state/software_setup.md`
2. Link to official FPP documentation for details
3. Provide quick-start instructions for the feature
4. Note minimum FPP version required
5. Add to appropriate section (group related features)

### Task: Add Troubleshooting Entry

**When**: Common issue reported or discovered

**Steps**:
1. Add to relevant guide (hardware, software, or integration)
2. Format as:
   - **Symptom**: Clear description of the problem
   - **Diagnosis Steps**: How to identify the issue
   - **Common Causes**: List of likely causes
   - **Solutions**: Step-by-step fixes
3. Include any relevant log output or error messages
4. Link to related external resources if helpful

### Task: Update Master Plan with Decision

**When**: Major project decision is made

**Steps**:
1. Open `master_plan.md`
2. Add new dated entry to "Decision Log" section:
   ```markdown
   ### Entry #: YYYY-MM-DD - [Decision Title]
   **Decision**: [What was decided]
   
   **Rationale**: [Why this decision]
   
   **Trade-offs Accepted**: [What we're giving up]
   
   **Alternatives Considered**: [What else was evaluated]
   
   **Next Steps**: [What happens next]
   ```
3. Update phase status if decision affects timeline
4. Increment version in Version History table

### Task: Start New Project Phase

**When**: Moving from one phase to next (e.g., Planning → Hardware Procurement)

**Steps**:
1. Update `master_plan.md`:
   - Mark previous phase status as ✅ Completed
   - Update current phase status to ✅ In Progress
   - Add dated entry summarizing phase transition
   - Note any discovered blockers or lessons learned
2. Update version number and history
3. Consider updating README.md with current status

---

## External Integration: haconfiguration Repository

### Purpose

The `richardthorek/haconfiguration` repository contains the actual Home Assistant configuration YAML files that integrate with this LED lighting system.

### What Belongs Where

**home-lighting repository** (this repo):
- Documentation and guides
- Decision rationale and architecture
- Shopping lists and hardware specifications
- Installation instructions
- Troubleshooting guides
- Reference examples

**haconfiguration repository**:
- Actual Home Assistant configuration files
- `packages/christmas_lights.yaml` - Complete HA integration
- Automations, scripts, scenes
- Lovelace dashboard configurations
- Secrets (with proper gitignore)

### Cross-Repository Updates

**When updating HA integration docs here**:
- Reference specific files in haconfiguration repo
- Show example YAML that matches actual implementation
- Note in `docs/current_state/home_assistant_integration.md` what should be added to haconfiguration
- Provide link to haconfiguration repo

**Example Reference**:
```markdown
Add the following to your `richardthorek/haconfiguration/packages/christmas_lights.yaml` file:

[YAML example here]

See the full implementation at: https://github.com/richardthorek/haconfiguration
```

---

## Response to Common Queries

### "How do I...?"

**If implementation guide exists**: Link to specific section in relevant doc

**If not documented**: 
1. Answer the question thoroughly
2. Suggest adding to documentation
3. Provide location where it should be added

### "Why did we choose...?"

**Check `master_plan.md` Decision Log** first - rationale should be documented there

**If not in Decision Log**: Provide answer and suggest adding decision entry

### "This link is broken / This information is outdated"

1. Acknowledge the issue
2. Update the affected documentation
3. Verify related links/information
4. Update "Updated: YYYY-MM-DD" date
5. Commit with clear message: "Fix: Update broken link to [resource]" or "Update: Refresh pricing for [component]"

### "Can I use [alternative hardware/software]?"

1. Evaluate compatibility with overall architecture
2. Check if trade-offs are acceptable
3. If viable alternative:
   - Add to decision checklist with pros/cons
   - Reference in shopping list or setup guide
   - Consider adding as alternative path in guides
4. If not compatible, explain why and suggest what would need to change

---

## Commit Message Conventions

Use conventional commit format:

**Types**:
- `docs:` - Documentation changes
- `fix:` - Bug fixes in docs, broken links, typos
- `feat:` - New guides, sections, or features
- `update:` - Refresh of existing information (prices, versions)
- `chore:` - Meta tasks (updating instructions, structure)

**Examples**:
- `docs: Add troubleshooting section for MQTT connection issues`
- `fix: Update broken link to Falcon PiCap product page`
- `update: Refresh shopping list pricing as of 2025-12`
- `feat: Add automation example for weather-reactive lighting`
- `docs: Document xLights sequence export to FPP process`

**Scope** (optional):
- `docs(hardware):` - Hardware setup guide changes
- `docs(ha):` - Home Assistant integration changes
- `update(shopping):` - Shopping list updates

---

## Key Principles

1. **Single Source of Truth**: `master_plan.md` is authoritative for strategy and decisions
2. **Actionable Documentation**: Every guide should enable a user to complete a task
3. **External References**: Link to official docs, don't duplicate them
4. **Beginner-Friendly**: Assume reader is DIY-capable but not expert
5. **Safety First**: Always include relevant safety warnings
6. **Keep Current**: Update dates and verify accuracy periodically
7. **Show, Don't Just Tell**: Include examples, screenshots, code snippets

---

## Resources for Copilot

**When assisting with documentation**:

**External Reference Docs**:
- FPP: https://falconchristmas.github.io/FPP/
- xLights: https://xlights.org/docs/
- Home Assistant: https://www.home-assistant.io/docs/
- Falcon Hardware: https://pixelcontroller.com/
- MQTT: https://www.home-assistant.io/integrations/mqtt/

**Community Forums** (for troubleshooting research):
- AusChristmasLighting: https://auschristmaslighting.com/
- FalconChristmas: https://falconchristmas.com/forum/
- Home Assistant Community: https://community.home-assistant.io/

**Verify Technical Claims**:
- When unsure about specifications, reference official product manuals
- When providing electrical calculations, double-check formulas
- When suggesting YAML, validate syntax

---

## Version

**Copilot Instructions Version**: 1.0  
**Last Updated**: 2025-12-24  
**Applies to**: home-lighting repository structure as of December 2024

---

**Questions?** Refer to `master_plan.md` for project overview or `docs/current_state/getting_started.md` for implementation starting point.
