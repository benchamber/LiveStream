# LiveStream — Road to 70.3 overlay

IRL stream overlay for **Moblin** (iPhone). One page: `overlay.html`.

- The page draws the frame: title badge, live timer, race countdown, gel reminder, chat, and four empty stat cards.
- The live numbers (heart rate, pace/speed, distance, elevation gain) come from **Moblin's own Text widgets**, placed over the empty cards. A browser widget only receives chat from Moblin, not sensor data.

Public address (once GitHub Pages is on): `https://benchamber.github.io/LiveStream/overlay.html`

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

### 1. Heart-rate source (Garmin)

1. On the watch: turn on **heart-rate broadcast** (Garmin: *Broadcast Heart Rate*, or *Broadcast During Activity* in the HR sensor settings).
2. In Moblin: **Settings → Workout devices** → add a device, scan, and pick the watch.
3. Give the device a **lowercase name with no spaces**, e.g. `garmin`. Moblin reads `{heartrate:…}` in lowercase, so a lowercase name avoids a mismatch.

### 2. Browser widget (the overlay)

**Settings → Widgets → +** → type **Browser**:

- URL: your run or bike address (see *Final addresses*)
- Width **1920**, Height **1080**
- **Moblin access: ON** (without it, chat stays empty)
- Mode: leave the default (*Periodic, audio and video*)

### 3. Four Text widgets (live numbers)

Add one **Text** widget per card. Replace `garmin` with your device name.

| Card | Run text | Bike text |
|---|---|---|
| 1 Heart rate | `{heartrate:garmin}` | `{heartrate:garmin}` |
| 2 Pace / Speed | `{runningpace:garmin}` ⚠ see below | `{speed:km/h}` |
| 3 Distance | `{distance:km}` | `{distance:km}` |
| 4 Elevation gain | `{altitudeascent:m}` | `{altitudeascent:m}` |

Style: transparent background, bold, large (around 64 on the 1920×1080 frame). Use monospace digits if offered.

⚠ `{runningpace:…}` needs a sensor that broadcasts running pace (a foot pod). A Garmin watch normally broadcasts heart rate only, so the card would show `-`. Without a foot pod, use `{speed:km/h}` (GPS) on card 2 instead.

### 4. Scene

**Settings → Scenes →** your stream scene → add the camera, the Browser widget and the 4 Text widgets. The Text widgets must be drawn above the Browser widget, which must be above the camera.

Positions (alignment **top left**):

| Widget | X | Y |
|---|---|---|
| Browser | 0 | 0 (size 100 %) |
| Text, card 1 | 3.8 | 86.7 |
| Text, card 2 | 20.2 | 86.7 |
| Text, card 3 | 36.7 | 86.7 |
| Text, card 4 | 53.1 | 86.7 |

Tip: first set the browser URL with `&guide=1` added. The dashed boxes show where each number goes. Line them up, then remove `&guide=1`.

If the numbers are hidden behind the cards, move the Text widgets above the Browser widget in the scene list.

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
