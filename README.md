# RDS‑Crafting (Tablet Crafting Bench)

RDS‑Crafting is a QB‑Core crafting system with:

- Tablet‑style NUI interface (yellow themed).
- Support for **qb-inventory** and **ox_inventory**.
- Active Crafts bar with **per‑item timers** and **multi‑crafting** (craft several recipes at once).
- Job‑locked recipes (police, ambulance, mechanic).
- Multiple categories (Tools, Medical, Weapons, Electronics, Materials/Refining, Parts).
- Responsive grid: up to 5 items per row on wide screens.

---

## Features

### UI / Tablet

- RDS‑styled **tablet** centered on screen:
  - Header with title and ESC hint.
  - **Tabs across the top** (Tools, Weapons, Medical, Electronics, Parts, Materials).
  - **Active Crafts** horizontal bar with:
    - Tiny pill for each active craft.
    - Live time remaining.
    - Yellow → green progress bar.
    - Left/right arrow buttons to scroll if there are many crafts.
  - Main area is a **grid of item cards**:
    - Shows name, description.
    - Progress section with timer.
    - Quantity input (1–50).
    - Required materials with live red/green color based on your inventory.
    - Yellow “CRAFT” button that becomes green “COLLECT” when done.

### Crafting Logic

- **Two‑step crafting**:
  1. Click **CRAFT**:
     - Server checks you have enough materials.
     - Required materials are **removed immediately** and “reserved”.
     - Client starts a timer and adds an entry to Active Crafts.
  2. When the timer finishes, the button becomes **COLLECT** or the Active Crafts pill turns green:
     - Clicking COLLECT calls the server to **give the item(s)**.
- **Multiple crafts at once**:
  - Each card gets its own `craftId`.
  - You can start Hammer, Bandage, Phone etc. simultaneously; all show in Active Crafts.
  - No global `isCrafting` lock – each card is independent.

### Inventory & Target

- Auto‑detects inventory:
  - `ox_inventory` if running.
  - Else `qb-inventory`.
- Auto‑detects target:
  - `ox_target` or `qb-target`, or falls back to **E key** interaction.
- Bench prop: `gr_prop_gr_bench_01b` spawned at configured location.

### Job Locks

- **Bench lock (optional)**:
  - `jobs = {"mechanic"}` on a `CraftingLocations` entry = only those jobs see/use that bench.
- **Item lock (per recipe)**:
  - Each recipe can have `jobs = {"police"}`, `{"ambulance"}`, `{"mechanic"}`, etc.
  - Items without `jobs` are **public**.
- Client sends only **allowed items and categories** to NUI for that player.

### Categories & Example Items

The default `config.lua` ships with:

- **Tools**
  - Public: Hammer, Screwdriver Set, Wrench, Crowbar, Pliers, Lockpick.
  - Mechanic only: Drill, Blowtorch, Toolbox.
  - Police only: Advanced Lockpick, Handcuffs, Bodycam.
- **Medical**
  - Public: Bandage, Painkillers, Antibiotics, Large Bandage, Gauze, Saline, Vitamins, Medical Gloves.
  - EMS only: First Aid, Medkit, Splint, Morphine.
- **Weapons**
  - Police ammo: pistol_ammo, smg_ammo, rifle_ammo.
  - Public: Weapon Cleaning Kit.
  - Police weapons & attachments: Service Pistol, Carbine Rifle, Flashlight, Suppressor.
- **Electronics**
  - Public: Phone, Radio, Laptop, Tablet, Battery Pack, CCTV Camera.
  - Police/Hacker tier: GPS Tracker (police), Hacking Device (illegal).
- **Materials (Refining)**
  - Public: Steel, Copper, Plastic, Electronics, Rubber, Glass, Aluminum, Gunpowder.
- **Parts (Vehicle)**
  - Mechanic only: Carbattery, Turbo, Transmission, Engine Block, Brake Pads.

You can of course change/extend these.

---

## Requirements

- **QB‑Core framework**
- One of:
  - `qb-inventory`
  - or `ox_inventory`
- Optional (but recommended) target:
  - `ox_target`
  - or `qb-target`
- Any items referenced in `config.lua` must exist in `qb-core/shared/items.lua` or your inventory system (e.g. `metalscrap`, `chemicals`, `gunpowder`, `sulfur`, etc.).

---
