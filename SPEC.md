# Minecraft Clone - Specification Document

## 1. Project Overview

**Project Name:** VoxelCraft  
**Project Type:** Browser-based 3D voxel game  
**Core Functionality:** A simplified Minecraft-inspired game featuring procedural terrain generation, first-person movement, block placing/breaking, day-night cycle, and inventory system  
**Target Users:** Casual gamers looking for a nostalgic block-building experience in the browser

---

## 2. UI/UX Specification

### Layout Structure

**Game Screen:**
- Full viewport 3D canvas (100vw × 100vh)
- HUD overlay with crosshair at center
- Hotbar at bottom center (9 slots)
- Debug info panel (top-left, optional toggle)
- Title/instructions overlay on start

**Responsive Behavior:**
- Desktop-only (requires keyboard + mouse)
- Minimum resolution: 1024×768

### Visual Design

**Color Palette:**
- Sky Day: `#87CEEB` (light sky blue)
- Sky Night: `#0a0a1a` (deep navy)
- Fog Day: `#c8e0f0`
- Fog Night: `#1a1a2e`
- UI Background: `rgba(0, 0, 0, 0.6)`
- UI Border: `#444444`
- UI Text: `#ffffff`
- Selection Highlight: `rgba(255, 255, 255, 0.3)`
- Hotbar Selected: `#ffffff` border with glow

**Typography:**
- Font Family: `"Silkscreen", "Press Start 2P", monospace` (pixel-style)
- HUD Text Size: 14px
- Title Size: 32px
- Instructions: 12px

**Block Textures (procedural/colors):**
- Grass: Top `#4a9c2d`, Side `#6b4423` with green top edge, Bottom `#5c3d1e`
- Dirt: `#5c3d1e`
- Stone: `#7a7a7a`
- Wood (Oak Log): `#6b4423` with bark pattern
- Leaves: `#2d6b1a` (semi-transparent)
- Sand: `#d4b896`
- Water: `#3498db` (transparent, animated)
- Coal Ore: `#7a7a7a` base with `#1a1a1a` specks
- Iron Ore: `#7a7a7a` base with `#b87333` specks
- Gold Ore: `#7a7a7a` base with `#ffd700` specks
- Diamond Ore: `#7a7a7a` base with `#00ffff` specks

### Components

**Crosshair:**
- White cross, 2px thick, 20px arms
- Centered on screen

**Hotbar:**
- 9 slots, each 50×50px
- 4px gap between slots
- Selected slot has white glowing border
- Block icon displayed in each slot
- Number key indicators (1-9)

**Instructions Overlay:**
- Semi-transparent black background
- White pixel font text
- Lists controls: WASD, Space, Click, 1-9, E
- "Click to Start" prompt
- Fades out on game start

**Debug Panel (F3 to toggle):**
- Position: top-left
- Shows: FPS, chunk count, player coords
- Monospace font, green text on black

---

## 3. Functionality Specification

### Core Features

**1. Terrain Generation**
- Chunk-based infinite terrain (16×16×64 blocks per chunk)
- Simplex/Perlin noise for heightmap
- Biomes: Plains (grass), Desert (sand), Mountains (stone)
- Trees: Randomly placed on grass blocks (oak logs + leaves)
- Caves: 3D noise carving (optional, may limit for performance)

**2. Player Movement**
- First-person camera
- WASD: Movement (forward/back/strafe)
- Space: Jump (with gravity)
- Shift: Sneak/slow walk
- Mouse: Look around (pointer lock)
- Movement speed: 5 units/sec
- Jump height: 1.2 blocks
- Gravity: 25 units/sec²

**3. Block Interaction**
- Left Click: Break block (instant, drops item)
- Right Click: Place block (from selected hotbar slot)
- Raycasting: 5 block reach distance
- Block highlight: Wireframe on hover
- Block break animation: Crack overlay (optional)

**4. Inventory System**
- 9-slot hotbar (keys 1-9 to select)
- E key: Toggle inventory screen (shows all block types)
- Creative-style: Unlimited blocks
- Default blocks: Grass, Dirt, Stone, Wood, Leaves, Sand, Coal, Iron, Gold, Diamond

**5. Day-Night Cycle**
- Full cycle: 10 minutes real-time
- Sun/moon movement across sky
- Sky color interpolation
- Light level affecting visibility
- Ambient light adjustment

**6. Physics**
- Player collision with blocks
- Simple AABB collision detection
- No fall damage (for simplicity)
- Water blocks: Swim + sink slowly

### User Interactions and Flows

**Game Start:**
1. Page loads → Show title + instructions overlay
2. User clicks → Request pointer lock
3. Pointer locked → Hide overlay, start game loop

**Gameplay Loop:**
1. Move/look around
2. Approach blocks → See highlight
3. Left-click → Break block (add to inventory if survival)
4. Right-click → Place selected block
5. Press 1-9 → Select hotbar slot
6. Press E → Open inventory
7. Press F3 → Toggle debug
8. Press Escape → Show pause menu (optional)

### Edge Cases

- Prevent placing blocks inside player
- Clamp player position to world bounds
- Handle chunk loading/unloading smoothly
- Graceful fallback if WebGL not supported

---

## 4. Technical Implementation

**Rendering Engine:** Three.js (via CDN)
- WebGL renderer
- Perspective camera (FOV: 75)
- Directional light (sun) + ambient light

**Performance Targets:**
- 60 FPS on modern hardware
- Render distance: 4 chunks
- View distance adjustable

**File Structure:**
```
/minecraft/
  index.html    - Single file containing all code
  SPEC.md       - This specification
```

---

## 5. Acceptance Criteria

### Visual Checkpoints
- [ ] 3D terrain renders with varied height
- [ ] Player can look around smoothly
- [ ] Blocks are clearly distinguishable by color
- [ ] Sky changes color during day/night
- [ ] Hotbar displays correctly at bottom
- [ ] Crosshair centered on screen

### Functional Checkpoints
- [ ] WASD moves player in correct directions
- [ ] Mouse controls camera rotation
- [ ] Space makes player jump
- [ ] Left-click breaks targeted block
- [ ] Right-click places selected block
- [ ] Number keys 1-9 change selected block
- [ ] Day/night cycle progresses over time

### Performance Checkpoints
- [ ] Maintains 30+ FPS with default settings
- [ ] No visible chunk loading stutter
- [ ] Smooth mouse look without jitter
