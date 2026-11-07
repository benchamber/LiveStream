# Livestream Moblin — Road to 70.3 overlay

GitHub repository: `benchamber/LiveStream`. This name is part of the public address, so keep it if you rename the project elsewhere.

IRL stream overlay for **Moblin** (iPhone). One page: `overlay.html`.

- The page draws the frame: title badge, live timer, race countdown, gel reminder, chat, and four empty stat cards.
- The live numbers (heart rate, pace/speed, distance, elevation gain) come from **Moblin's own Text widgets**, placed over the empty cards. A browser widget only receives chat from Moblin, not sensor data.

Public address: `https://benchamber.github.io/LiveStream/overlay.html`

## URL settings

Everything is set in the address. You never need to edit the code.

| Parameter | What it does | Example |
|---|---|---|
| `title` | Badge text | `title=ROAD%20TO%2070.3` |
| `races` | Races as `name|YYYY-MM-DD`, separated by `;`. The next upcoming race is shown automatically. | `races=Race%20A|2027-06-21;Race%20B|2027-08-15` |
| `race` | Force race number N from the list (1 = first) | `race=2` |
| `mode` | `run` (default) or `bike`: changes the card labels | `mode=bike` |
| `gel` | Gel reminder every N minutes. Off unless set | `gel=25` |
| `accent` | Accent colour, no `#` | `accent=C8F03C` |
| `chat` | `0` hides chat | `chat=0` |
| `hrmax`, `hrrest` | Heart-rate zone scale under the heart-rate number. With `hrrest`, zones use heart-rate reserve (Karvonen) at 50/60/70/80/90 %; without it, % of max | `hrmax=180&hrrest=45` |
| `zones` | Or give the 5 zone lower bounds yourself (Z1..Z5) | `zones=113,126,140,153,167` |
| `preview` | `1` shows sample numbers and chat (for a desktop browser) | `preview=1` |
| `guide` | `1` shows where each Moblin text widget goes (X/Y in %) | `guide=1` |

In the address, a space is written `%20` and `|` can be written `%7C`.

## Moblin setup

### Fastest way: import the prepared settings

Claude can build a Moblin settings file from your own export, with everything below already set up. **That file contains stream keys and passwords, so it never goes into this public repository.**

1. Moblin → **Settings → Import and export settings → Export**. Keep that file as a backup.
2. Give it to Claude. You get back a file with the **Course** and **Vélo** scenes.
3. Moblin → **Import and export settings → Import from file**. The import **replaces** all settings, which is why it starts from your own export. It is not available while live or recording.

The manual setup below is the same thing done by hand.

### 1. Sensors (Settings → Workout devices)

| Device | Name in Moblin | Used for |
|---|---|---|
| Garmin watch (heart-rate broadcast on) | `Garmin watch` | `{heartrate:garmin watch}` |
| Garmin HRM 600 chest strap | `HRM 600` | `{runningpace:hrm 600}` (run pace) |

Moblin ignores upper and lower case when matching these names.

On the watch, turn on **heart-rate broadcast** (*Broadcast Heart Rate*, or *Broadcast During Activity*).

### 2. Two scenes: Course (run) and Vélo (bike)

Each scene has its own Browser widget, followed by the 4 Text widgets. The widget listed last is drawn on top, so the Browser widget comes first.

**Browser widget:** URL = the run or bike address (see *Final addresses*), Width **1920**, Height **1080**, **Moblin access ON**, default mode.

**Text widgets:**

| Card | Course | Vélo |
|---|---|---|
| 1 Heart rate | `{heartrate:garmin watch}` | `{heartrate:garmin watch}` |
| 2 Pace / Speed | `{runningpace:hrm 600}` | `{speed:km/h}` |
| 3 Distance | `{distance}` (fallback `{distance:m}`) | `{distance}` (fallback `{distance:m}`) |
| 4 Elevation gain | `{altitudeascent:m}` | `{altitudeascent:m}` |

Style: font **DIN Condensed** (Bold), size **64**, white, background fully transparent, corner radius **0**. Moblin adds the unit itself for distance, elevation and speed (`12.4 km`, `86 m`, `31 km/h`). Heart rate and pace have their unit in the card header (BPM, MIN/KM).

Distance: `{distance:km}` rounds to whole kilometres (Moblin prints 0 decimals with an explicit unit). Plain `{distance}` uses Moblin's automatic format, which should show decimals. If it still shows whole numbers, use `{distance:m}` (metres).

**Positions** (alignment **top left**, size 100). These include the ~15 px left padding Moblin adds around text:

| Widget | X | Y |
|---|---|---|
| Browser | 0 | 0 |
| Text, card 1 | 2.97 | 82.59 |
| Text, card 2 | 19.43 | 82.59 |
| Text, card 3 | 35.89 | 82.59 |
| Text, card 4 | 52.34 | 82.59 |

To check, add `&guide=1` to the browser URL. The dashed boxes show where each text widget goes. Nudge Y if the font sits a little high or low, then remove `&guide=1`.

**Camera and front-camera PiP:**

- Scene camera (Course and Vélo): **Back**, the wide (1×) camera. It shows what you see.
- Front camera picture-in-picture: add the **Front** video-source widget **last** in each scene, so it is drawn on top. Alignment **Bottom right**, X **2.5**, Y **4.44**, Size **25**. That makes a 480×270 landscape window in the bottom-right corner, level with the bottom of the cards. In the widget: crop off, rounded corners on.
- Running both cameras at once uses more battery and makes the phone warmer.

**Location → Reset when going live: ON** makes distance and elevation start at 0 on each stream.

### Pace in min/km

Moblin has a single pace code, `{runningpace:DEVICE}`. It reads a Bluetooth *Running Speed and Cadence* sensor, **not GPS**. The watch broadcasts heart rate only, so pace comes from the HRM 600 if it shares pace over Bluetooth. That still needs to be confirmed on a test run. If the card shows `-`, the strap does not, and GPS speed (`{speed:km/h}`) is the fallback. Also check that the pace shows min/km and not min/mile.

## Final addresses

Race: **IRONMAN 70.3 Maine**, Augusta, Sunday **2027-07-25**. To add races later, separate them with `;` in `races=`.

Run:

```
https://benchamber.github.io/LiveStream/overlay.html?races=Ironman%2070.3%20Maine%7C2027-07-25&hrmax=180&hrrest=45
```

Bike:

```
https://benchamber.github.io/LiveStream/overlay.html?races=Ironman%2070.3%20Maine%7C2027-07-25&hrmax=180&hrrest=45&mode=bike
```

Heart-rate zones (max 180, resting 45, heart-rate reserve method): Z1 113–125 · Z2 126–139 · Z3 140–152 · Z4 153–166 · Z5 167–180. The scale is fixed: Moblin does not pass heart rate to the page, so the current zone cannot light up. Viewers read the zone from the live number.

For lining up the text widgets, add `&guide=1` to the end, then remove it.

If the repository is renamed, replace `LiveStream` in these addresses with the new name.
