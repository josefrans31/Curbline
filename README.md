# Curbline

A 3D parking and city driving game in a single HTML page (Three.js from cdnjs, no build step).

Play it at https://curbline.netlify.app. Pushes to `main` deploy there automatically through Netlify.

## What is in the game

- **Career**: license tests on the training ground, then paid parking jobs around the city.
- **Daily challenge**: one new job each day, the same for every player, with a streak bonus.
- **Free drive**: every car is unlocked, in an open city with traffic, pedestrians and police.
- **Garage**: 24 original cars with paint, wheels, stance and exhaust options.
- **City rules**: traffic lights, stop signs, a 50 km/h limit, seatbelts and turn signals. Patrol cars only act on what they can see (range, view cone and line of sight). Pull over and stop to take the ticket.
- **Car controls**: engine start and stop, seatbelt, horn, turn signals, hazards, headlights, drift mode, parking sensors and a rear camera.
- **Settings**: steering sensitivity, wheel or arrow steering on touch screens, time of day (morning, day, evening, night), police level (1 relaxed to 3 strict), parking assist, speed units, sound and shadows.
- Keyboard, touch and game controller support. The full list of keys and buttons is on the Settings screen.

Online free drive is switched off for now (`ONLINE_ENABLED` in `index.html`). The menu button stays visible and says so.

## Files

| File | What it is |
| --- | --- |
| `index.html` | The whole game: styles, markup and script |
| `privacy.html` | Standalone privacy policy, served at `/privacy`. The same text is on the Privacy policy screen inside the game, so change both together |
| `netlify.toml` | Tells Netlify to publish the repo root |

## Running locally

Open `index.html` in a browser, or serve the folder with any static server. Progress and settings are saved in the browser's local storage.
