# Wizard Beams - Technical Documentation

## Core Concept
A real-time magical beam struggle game where two wizards push energy back and forth. Players manage internal body nodes to generate mana and counter enemy beam types.

## Current Implementation

### Beam Struggle System
- Continuous tug-of-war: collision point moves based on mana differential
- Push force = (player active mana - enemy active mana)
- Win conditions: push collision point to enemy wizard OR reduce enemy HP to 0
- Beam visual thickness scales with active mana

### Beam Types (Rock-Paper-Scissors)
- **Spiral** (purple) beats Zap
- **Zap** (yellow) beats Pulse
- **Pulse** (orange) beats Spiral
- **Neutral** (grey) - no modifier, always available
- Active beam type provides +3/-3 modifier to push force based on matchup
- Beam types persist until switched
- 5 second cooldown on switching between attack types (Spiral/Pulse/Zap)
- No cooldown switching TO Neutral
- Beam type buttons are LOCKED until corresponding colored node is activated

### Node Network System
**16 total nodes in body layout:**
- 6 Functional Nodes (large, glowing): head, center, left_hand, right_hand, left_foot, right_foot
  - Can be assigned Spiral/Pulse/Zap/Dud (3 beam types randomly distributed, 3 are duds)
  - Provide +1 active mana when activated
  - Maximum 6 active mana from nodes
- 10 Pathway Nodes (small, grey): neck, shoulders, elbows, hips, knees
  - Connection points only, no mana contribution
  - Can be damaged to block travel paths

**Node States:**
- Dormant (starting state, dim)
- Activating (2 second progress circle when awareness arrives)
- Active (glowing, contributing mana)

**Awareness System:**
- Single point of focus that travels between nodes
- Starts at center node
- Must follow connected paths (pathfinding, cannot teleport)
- Travel time: 1 second between adjacent nodes
- Cannot click other nodes while awareness is traveling
- Activation progress pauses if awareness leaves, resumes on return

**Node Connections:**
- Head ? Neck ? Center
- Center ? L/R Shoulder ? L/R Elbow ? L/R Hand
- Center ? L/R Hip ? L/R Knee ? L/R Foot

### Enemy AI
- Fixed active mana value (currently 3)
- Randomly selects and commits to beam types
- Shows current beam type visually

### UI Layout
- Top 60%: Dramatic beam battle view (two wizards, energy collision point)
- Bottom left: Player's node network diagram (50% larger than initial design, positioned below player wizard)
- Bottom right: Beam type buttons (Spiral, Pulse, Zap, Neutral)
- Top corners: Current beam type and active mana displays

## Technical Notes
- Built with HTML5 Canvas
- Real-time rendering with game loop
- Uses pathfinding algorithm for awareness travel
- Deployed on Vercel via GitHub