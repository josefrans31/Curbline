# Curbline in Unity: rebuild plan

Status: **draft for Jose to confirm.** No Unity code is written yet. The web game (`index.html`) stays live and untouched while the Unity version is built.

Goal: ship Curbline on the App Store and Google Play, with better looking cars and city than the current code-built models, keeping the gameplay that already works.

---

## 1. What the current game has (feature inventory)

Everything below is in `index.html` today and is what the Unity version must match before release.

### Modes and menus
| Screen | What it does |
| --- | --- |
| Main menu | Coins, license status, current car. Buttons: Career, Daily challenge, Free drive, Online free drive (switched off, button stays), Garage, Settings. Footer: Privacy policy, Sound on/off, Reset progress |
| Career | Stage 1: five license courses on the training ground (L1 forward bay, L2 reverse bay, L3 slalom and stop, L4 parallel park, L5 tight turn). Passing all five issues the license (+500 coins) and unlocks Stage 2: eight city jobs (C1 mall lot, C2 reverse at Harbor Plaza, C3 downtown curb, C4 home driveway, C5 rush hour, C6 front-row reverse, C7 tight squeeze, C8 night shift) |
| Daily challenge | One job per calendar day, seeded from the date so everyone gets the same one (curb slot, mall bay, reverse bay or driveway, plus car, time of day and traffic). Pays 250 + 100 per star + streak bonus once a day; replays only pay for extra stars |
| Free drive | Pick any of the 24 cars (owned cars keep their mods), open city, no objective |
| Garage | 24 cars with prices. Tabs: Cars, Paint (16 colors, 5 finishes: gloss, metallic, pearl, matte, chrome), Interior (8 colors), Wheels (6 rim styles, 7 rim colors), Body and mods (4 body kits, 5 spoilers, 4 exhausts, stripes, underglow), Stance |
| Settings | Steering sensitivity, wheel or arrow steering on touch, time of day (morning, day, evening, night), police level 1 to 3, parking assist, km/h or mph, volume, shadows. Full key and controller list |
| Pause | Camera, shadows, sound, Settings, Change car, Quit, Restart, Resume |
| Result | Stars (3 for no touches, 2 for up to two, 1 otherwise), touches, time left, coins earned, Career / Retry / Next |
| Ticket | Offences seen, fines, "Pay and drive on" |
| Privacy policy | Same text as `privacy.html` |

### Driving
- Arcade car model with gears P, R, N, D, automatic creep, top speed and acceleration per car, steering lock per car, limited sideways grip so fast corners run wide, drift mode (toggle or hold) with skid marks and tire squeal.
- Engine start and stop (with crank), seatbelt, left and right signals, hazards, horn, headlights, exhaust pops on sporty classes.
- Cameras: chase, cockpit, top-down.
- Parking aids: parking sensors (four bars plus beeps), rear camera with guide lines in reverse, guide arrow and route to the target.
- Parking check: all four corners inside the box, heading within about 11 degrees, stopped, hold or shift to P to finish.
- Collisions with buildings, cones, parked cars, traffic and pedestrians count as touches.

### City and world
- 5 by 5 block grid (76 m blocks): training ground, shops, downtown, two malls (Eastgate Mall, Harbor Plaza), park, Maple Row suburb. Curb parking slots, mall bays, house driveways, invented shop names.
- Traffic cars on the road network, traffic lights on a cycle, stop signs, pedestrians.
- Time of day changes sky, sun, street lamps, lit windows. Night levels need headlights.
- Minimap.

### Police and rules (city only, not on the training ground)
- City speed limit 50 km/h. Offences: no seatbelt (40), no signal when turning (30), stop sign (60), red light (100), speeding (80), hitting a police car (120), failing to stop (150).
- Patrol cars only act on what they see: range, view cone and line of sight past buildings. Police level sets how many cars (2, 4, 7), how far they see and how much they forgive.
- Siren, "pull over" banner, stop to take the ticket, pursuit if you run.

### Controls
- Keyboard: W/S or arrows gas and brake, A/D steer, Space brake, I engine, B seatbelt, Q/E signals, Z hazards, H horn, X drift (Shift to hold), F/R/N/P gears, L lights, C camera, Esc pause.
- Touch: on-screen steering wheel (or arrows), gas and brake pedals, gear selector, buttons for belt, engine, signals, hazards, horn, drift, camera, lights.
- Game controller: sticks, triggers, bumpers for signals, face buttons, D-pad for gears.

### Sound (all synthesized in code today)
Engine (voice per cylinder count, rpm and throttle), engine start and stop, exhaust pops, tire squeal, horn, siren with distance falloff, indicator tick, seatbelt click, sensor beeps, impact thud, success chime, fail tone, UI click.

### Save data
Coins (start 300), owned cars, current car, per-car customization, stars per level, sound, shadows, camera, free drive car, settings, daily challenge state (date, stars, streak). Stored only on the device.

### Not carried over at launch
Online free drive (switched off on the web too). It can come back later as its own phase.

---

## 2. Unity version and template

- **Unity 6 LTS** (the newest version Unity Hub labels "LTS"; at the time of writing that is the Unity 6.3 LTS line, `6000.3.x`). LTS gets two years of fixes, which is what you want for a store release. Do not use a "Beta" or a non-LTS "Update" release.
- **Template: "Universal 3D"** (URP, Universal Render Pipeline). URP is Unity's mobile-friendly renderer and it runs well on a modest laptop. Do not pick HDRP (too heavy, not for phones).
- **License:** Unity Personal is free while revenue stays under Unity's threshold (currently $200,000 a year).
- Packages to add from the Package Manager in Phase 0: **Input System** (keyboard, touch and controller in one place), **Cinemachine** (cameras), **TextMeshPro** (comes with uGUI), **AI Navigation** is not needed (traffic follows our own lanes, like now).

### What the stores need (not Unity, but worth knowing now)
- Google Play: a developer account ($25 once). New personal accounts must run a closed test with at least 12 testers for 14 days before going public.
- App Store: an Apple Developer account ($99 a year) **and a Mac to do the final iOS build** in Xcode. Jose's computer is Windows, so the options are a Mac (a used Mac mini is enough) or a cloud build service that builds iOS for you (Unity Build Automation or Codemagic, both have free tiers). Android can be built and tested on the Windows laptop directly.
- Both stores need a privacy policy URL (the existing `privacy.html` on GitHub Pages works, updated for the app) and an age rating questionnaire. The game collects nothing, which keeps both simple.

---

## 3. Where the Unity project lives

**Recommended: a new GitHub repo, `curbline-unity`**, with Git LFS turned on for models, textures and audio, and Unity's standard `.gitignore` (so `Library/`, `Temp/`, `Builds/` are never committed). Reasons: a Unity project is large and binary heavy; keeping it out of this repo means GitHub Pages and Netlify keep serving the small web game exactly as now. Jose creates the empty repo (or says so and it can be added to this project). This plan file stays here as the hand-off.

### Folder layout inside the Unity project
```
Assets/
  _Curbline/
    Scenes/          Boot, Menu, Training, City
    Scripts/
      Core/          GameState, SaveSystem, Settings, Rng (same seeded RNG as the web game)
      Car/           CarController (ported physics), CarInput, CarLights, CarAudio, CarCustomizer
      Parking/       ParkingTarget, ParkCheck, Cones, HitCounter, LevelDefinition, LevelRunner
      City/          CityBuilder (5x5 grid), RoadGraph, Slots, Bays, Driveways, TrafficLights, StopSigns
      Traffic/       TrafficCar, TrafficManager, Pedestrian, PedestrianManager
      Police/        PatrolCar, Vision (cone and line of sight), Rules, Fines, Tickets
      Daily/         DailyChallenge
      UI/            Menu, Career, Garage, Settings, Pause, Result, Ticket, Hud, TouchControls, Minimap
      Camera/        CameraRig (chase, cockpit, top-down)
    Data/            ScriptableObjects: CarSpec (24), LevelDefinition (13), PolicePreset (3), TimeOfDay (4)
    Prefabs/         Cars, Props, Buildings, UI
    Art/             Models, Materials, Textures (from the asset packs below)
    Audio/
    Settings/        URP assets: one for phones, one for desktop testing
  ThirdParty/        Asset packs as imported, untouched
```

Key design choice: **car stats, levels, police levels and times of day become ScriptableObject data files**, copied from the tables in `index.html` (`CARS`, `LEVELS`, `COP_LV`, `TOD`). The code reads data, so tuning a car or a level is editing a field in the Inspector, not code.

**Physics choice:** port the existing arcade model in `physics()` (about 60 lines) to C# in `FixedUpdate`, on a Rigidbody for collisions, rather than using Unity's WheelColliders. It already feels right for parking, it is predictable on phones, and it means the Unity car drives like the web car from day one. WheelColliders can be tried later if more realism is wanted.

**UI choice:** uGUI (Canvas) with TextMeshPro for menus and the HUD. It is the most proven route for touch controls on phones.

---

## 4. Build order (each phase ends with something you can play)

| Phase | Result you can try | Main work |
| --- | --- | --- |
| **0. Setup** | An empty URP scene running on your Android phone | Install (section 6), create project from Universal 3D, add Input System and Cinemachine, mobile URP settings, first Android build over USB. Proving the phone build on day one avoids surprises later |
| **1. Drivable car** | Drive one car on a flat test lot with keyboard, controller and touch | Port `physics()` to `CarController`, gears P/R/N/D, chase camera, speedometer, touch pedals and steering wheel. Car is a placeholder model from a free pack |
| **2. Parking core** | The five license courses, with stars | Parking target box and the four-corner check, cones, touch counting, timer, result screen, license unlock. Training ground built from cones and simple props |
| **3. City** | Free drive around the 5x5 city | `CityBuilder` generates roads, lots, curb slots, bays and driveways from the same layout grid, dressed with modular city pieces. Colliders, minimap, guide arrow and route |
| **4. Living city** | Traffic, lights, stop signs, pedestrians | Port traffic lanes and intersections, signal cycle, pedestrians on sidewalks |
| **5. Career jobs** | City jobs C1 to C8 | Level data for the eight jobs, parked cars around targets, night level, parking sensors and rear camera |
| **6. Police** | Get pulled over | Patrol cars, vision cone and line of sight, rules, fines, siren, pursuit, ticket screen, police levels |
| **7. Cars and garage** | 24 cars to buy and customize | Pick and recolor 24 car models, coins and prices, paint and finish (material swaps), interior color, rims, spoilers, stance (ride height and offset). Body kits may be fewer than today, depending on the models |
| **8. Menus, save, daily** | The full game loop | Main menu, settings, pause, save to `persistentDataPath`, daily challenge (same seeded generator as the web), car controls (engine, belt, signals, hazards, horn, lights, drift) |
| **9. Look and sound** | The "looks better" pass | Lighting per time of day, baked lighting where possible, light post-processing (bloom, color grading), reflections on paint, recorded engine and effect sounds pitched by rpm |
| **10. Store release** | Builds in testers' hands | Performance pass on a low-end phone, app icon, splash, screenshots, store text, updated privacy policy, Google Play closed test, iOS build via Mac or cloud, TestFlight |

Phases 1 to 3 are the big ones. After Phase 3 the rest is mostly porting logic that already exists in `index.html`.

---

## 5. Art and sound to improve the look (free or cheap)

All of these are original assets with licenses that allow commercial games. They keep the rule that Curbline's cars are unbranded.

| Asset | Cost | Use |
| --- | --- | --- |
| Kenney **Car Kit** (kenney.nl) | Free, CC0 | Placeholder and filler cars, Phase 1 |
| Kenney **City Kit** (Roads, Commercial, Suburban) | Free, CC0 | Roads, buildings, houses for the first city pass |
| Quaternius car and city packs (quaternius.com) | Free, CC0 | More low-poly cars and props |
| Synty **POLYGON City** or **POLYGON Town** (Unity Asset Store) | Paid, often on sale | One consistent stylized city with cars, people and props; the quickest way to a polished look |
| Unity Asset Store "free" filter, search "low poly cars" / "city" | Free | Extra cars; check each license and that cars carry no real badges |
| **Poly Haven** (polyhaven.com) | Free, CC0 | Sky HDRIs per time of day, asphalt and concrete textures |
| **ambientCG** (ambientcg.com) | Free, CC0 | Road, curb, paint and ground textures |
| **Mixamo** (Adobe account) | Free | Pedestrian characters and walk animations |
| **Sonniss GDC audio bundles** | Free, royalty free | Engine, tires, horn, impacts, ambience |

Suggested art direction: clean stylized low-poly (like Synty), not photoreal. It looks good on phones, runs fast on modest hardware, and it is the style asset packs do best. Mixing packs from different artists is the most common way games end up looking cheap, so pick one main city pack and one main car source.

---

## 6. Install steps (Jose does these on the Windows laptop)

About 15 to 25 GB of disk is needed. Use an SSD if the laptop has one.

1. **Unity Hub.** Go to unity.com/download, download Unity Hub for Windows, install it, open it and sign in (create a free Unity account if needed). When it asks about a license, choose **Unity Personal**.
2. **The editor.** In Hub open **Installs**, click **Install Editor**, and pick the newest version marked **LTS** under Unity 6.
3. **Modules.** On the module screen for that version, tick:
   - **Android Build Support**, and under it **OpenJDK** and **Android SDK & NDK Tools**
   - **iOS Build Support** (lets you set up and export the iOS project from Windows; the final build still needs a Mac or a cloud build)
   - **Microsoft Visual Studio Community**: leave this **unticked** if Visual Studio is already installed, tick it if not
   Then accept the Android license and install. Modules can be added later from Installs, the gear icon, **Add modules**.
4. **Visual Studio's Unity workload.** Unreal used the "Game development with C++" workload; Unity needs a different one. Open **Visual Studio Installer** from the Start menu, click **Modify** on your Visual Studio, tick **Game development with Unity**, click **Modify** to install.
5. **Link them.** Create a test project (Hub, **Projects**, **New project**, template **Universal 3D**). In Unity open **Edit > Preferences > External Tools** and set **External Script Editor** to Visual Studio. Double-clicking a C# script should now open it in Visual Studio.
6. **Phone (Android).** On the phone, open Settings, About phone, tap Build number seven times to turn on Developer options, then turn on **USB debugging**. Plug it in. In Unity, **File > Build Profiles**, choose **Android**, click **Switch Platform**, then **Build And Run**.

Tips for a modest laptop: keep only one Unity project open, close Hub while working, set the editor's Game view to a phone resolution, and leave the Scene view on lower quality. The first project open takes several minutes while Unity imports; later opens are quick.

---

## 7. Decisions for Jose

1. Confirm this plan (or change anything) before porting starts.
2. OK to use a new `curbline-unity` repo (recommended) instead of putting the Unity project in this repo.
3. iOS route: get a Mac later, or use a cloud build service. Nothing is needed until Phase 10.
4. Art budget: free packs only, or one paid pack (for example Synty POLYGON City) for a consistent look.
