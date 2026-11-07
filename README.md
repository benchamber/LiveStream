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
| `gel` | Gel reminder every N minutes (`0` hides it) | `gel=30` |
| `accent` | Accent colour, no `#` | `accent=C8F03C` |
| `chat` | `0` hides chat | `chat=0` |
| `preview` | `1` shows sample numbers and chat (for a desktop browser) | `preview=1` |
| `guide` | `1` shows where each Moblin text widget goes (X/Y in %) | `guide=1` |

In the address, a space is written `%20` and `|` can be written `%7C`.

## Moblin setup

### 1. Sensors

1. On the Garmin watch, turn on **heart-rate broadcast**. On Garmin it is called *Broadcast Heart Rate*, or *Broadcast During Activity* in the heart-rate sensor settings.
2. In Moblin, go to **Settings → Workout devices**, add a device, scan, and pick the watch.
3. Name it **`garmin watch`**, in lowercase. Moblin reads the name inside `{heartrate:…}` in lowercase, so a lowercase name avoids a mismatch. The space is fine.
4. Running pace needs a **foot pod** that broadcasts Bluetooth *Running Speed and Cadence*. Add it the same way and give it a lowercase name, e.g. `foot pod`. See *Pace in min/km* below.

### 2. Two scenes: Run and Bike

Make two scenes in **Settings → Scenes**. Each one has the camera, its own Browser widget and its own 4 Text widgets. The Text widgets must be drawn above the Browser widget, which must be above the camera.

**Browser widget** (**Settings → Widgets → +** → type **Browser**). Make one for each sport:

- URL: the run or bike address (see *Final addresses*)
- Width **1920**, Height **1080**
- **Moblin access: ON** (without it, chat stays empty)
- Mode: leave the default (*Periodic, audio and video*)

**Text widgets.** Make one per card:

| Card | Run scene | Bike scene |
|---|---|---|
| 1 Heart rate | `{heartrate:garmin watch}` | `{heartrate:garmin watch}` |
| 2 Pace / Speed | `{runningpace:foot pod}`: pace (min/km) | `{speed:km/h}`: speed |
| 3 Distance | `{distance:km}` | `{distance:km}` |
| 4 Elevation gain | `{altitudeascent:m}` | `{altitudeascent:m}` |

Cards 1, 3 and 4 are the same in both scenes. You can create them once and add them to both scenes.

Style: transparent background, bold, large (around 64 on the 1920×1080 frame). Use monospace digits if offered.

**Positions** (alignment **top left**), the same in both scenes:

| Widget | X | Y |
|---|---|---|
| Browser | 0 | 0 (size 100 %) |
| Text, card 1 | 3.8 | 86.7 |
| Text, card 2 | 20.2 | 86.7 |
| Text, card 3 | 36.7 | 86.7 |
| Text, card 4 | 53.1 | 86.7 |

Tip: first set the browser URL with `&guide=1` added. The dashed boxes show where each number goes. Line them up, then remove `&guide=1`.

If the numbers are hidden behind the cards, move the Text widgets above the Browser widget in the scene list.

### Pace in min/km

Moblin has only one pace code, `{runningpace:DEVICE}`. Its value comes from a Bluetooth foot pod (Running Speed and Cadence), **not from the phone's GPS**. A Garmin watch broadcasts heart rate only, so without a foot pod the pace card shows `-`.

- **With a foot pod:** use `{runningpace:foot pod}`. Check on the first run that it shows min/km and not min/mile. Moblin may follow the phone's unit settings.
- **Without a foot pod:** GPS only gives speed: `{speed:km/h}`. The run card label still says "ALLURE / min/km", so the label would need changing too.

## Final addresses

Race: **IRONMAN 70.3 Maine**, Augusta, Sunday **2027-07-25**. To add races later, separate them with `;` in `races=`.

Run:

```
https://benchamber.github.io/LiveStream/overlay.html?races=Ironman%2070.3%20Maine%7C2027-07-25
```

Bike:

```
https://benchamber.github.io/LiveStream/overlay.html?races=Ironman%2070.3%20Maine%7C2027-07-25&mode=bike
```

For lining up the text widgets, add `&guide=1` to the end, then remove it.

If the repository is renamed, replace `LiveStream` in these addresses with the new name.
