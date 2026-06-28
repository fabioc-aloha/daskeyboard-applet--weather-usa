# Q Applet: Weather - USA (fabioc-aloha fork)

Displays a 4-day weather forecast for cities located in the USA on a Das
Keyboard Q series.

> **About this fork.** This is a maintenance fork of the original
> [`daskeyboard/daskeyboard-applet--weather-usa`](https://github.com/daskeyboard/daskeyboard-applet--weather-usa),
> which has been in maintenance mode since January 2023 (the only open
> issue, [#23 "NWS zone changes"](https://github.com/daskeyboard/daskeyboard-applet--weather-usa/issues/23),
> has been unanswered for 2 years). This fork ships targeted fixes for
> users who want the original applet to keep working without switching.
>
> **Looking for the next-generation version?** A complete rewrite with
> NWS grid-based forecasts (no zone drift), live city search, severe-weather
> alerts, and click-to-open NWS pages now lives at
> [**Q-Weather-USA-Pro**](https://github.com/fabioc-aloha/Q-Weather-USA-Pro).
> New development happens there.

## Goal of this fork

Keep the original applet working for existing users while the rewrite
(`Q-Weather-USA-Pro`) matures. Specifically:

- Patch deprecated dependencies as they accumulate security issues
- Reduce the "missing city" friction without changing the install path
- Preserve the existing zone-id config so users don't lose their setup

If you're a new user, install `Q-Weather-USA-Pro` instead — it's better
on every axis. If you've already configured this applet and don't want to
re-do it, this fork is for you.

## What's new in this fork (vs upstream v1.0.12)

### v1.0.14

- **City-name aliases for 47 major US cities.** The original required you
  to search by county name (Charlotte is "Mecklenburg," Seattle is "King",
  etc.). Now you can search by `Charlotte`, `NYC`, `Vegas`, `SF`, etc. and
  get the right NWS zone with a label like `Mecklenburg (Charlotte), NC`.
  This single change addresses the recurring complaint pattern in the
  upstream issue tracker (Pittsburgh, Louisville, Tallahassee, Fort Wayne,
  Kansas City, San Antonio, "cities near me" — all closed-as-wontfix in
  upstream's one 2023 maintenance pass).

### v1.0.13

- **Replaced deprecated `request-promise`** with Node's built-in `https`
  module. No new runtime dependencies, no behavior change.
- **Removed the bogus `fs` npm dependency** (`fs` is a Node built-in; the
  npm package was a placeholder).
- **Upgraded mocha 5.2 → 10.7** for security and a modern test runner.

## Known issues this fork does NOT fix

- **NWS zone deprecation drift** (upstream issue #23). The applet uses the
  legacy `/zones/ZFP/{zoneId}/forecast` NWS endpoint, which breaks when NWS
  reorganizes zones (as they did for Oregon in 2024). Fixing this requires
  switching to the modern `/points` + `/gridpoints` API, which is a
  ground-up rewrite. That rewrite is `Q-Weather-USA-Pro`.
- **No severe-weather alerts.** Same reason.
- **`daskeyboard-applet` SDK still pulls in `request-promise` transitively.**
  Can't be fixed without forking the SDK itself.

## Example

Displays 4-day weather forecast on a Das Keyboard Q Series.
Each color corresponds to a weather change:  
clear or sunny (yellow), cloudy (purple), rainy (blue), storm (red), snow (white).

![Weather Forecast on a Das Keybaord Q](assets/image_keys.png "Q Weather Forecast")
![Weather Forecast on a Das Keybaord Q](assets/image_legend.png "Q Weather Forecast")

## Changelog

[CHANGELOG.MD](CHANGELOG.md)

## Installation

Requires a Das Keyboard Q Series: www.daskeyboard.com

The original applet installs through the Q Desktop marketplace
(<https://www.daskeyboard.com/q>). To run *this fork's* version on your
keyboard, sideload it into your Q Desktop install:

```powershell
# Quit Q Desktop first (right-click tray icon -> Quit)
cd "$env:USERPROFILE\.quio\v2\q_extensions"

# Back up the existing install (Q Desktop assigns a numeric id, often 11)
# Replace 11 with the folder name shown in your install if different.
Move-Item 11 11.bak

# Clone this fork into that slot
git clone https://github.com/fabioc-aloha/daskeyboard-applet--weather-usa.git 11
cd 11
yarn install --production   # or: npm install --omit=dev

# Restart Q Desktop
```

To roll back: delete the slot folder and restore the `.bak` backup.

## Finding your location

NWS forecast zones are named by **county or region**, not by city. So to find Charlotte, NC you would search for `Mecklenburg` (its county). To make this easier, the applet ships with a list of major-city aliases in [`city-aliases.json`](city-aliases.json) — searching the location picker for `Charlotte`, `Seattle`, `Vegas`, or `NYC` will find the right zone and display it as e.g. `Mecklenburg (Charlotte), NC`.

To add a city, append a new entry to [`city-aliases.json`](city-aliases.json):

```json
{ "zoneId": "NCZ071", "city": "Charlotte", "altNames": ["Queen City"] }
```

Find the zone id by looking at [`zones.json`](zones.json) (the full NWS catalog) or by searching the [NWS public-zone list](https://www.weather.gov/pimar/PubZone).

## Running tests

- `yarn test`

## Contributions

Pull requests welcome on this fork. For substantive new features or
architectural changes, please consider contributing to
[Q-Weather-USA-Pro](https://github.com/fabioc-aloha/Q-Weather-USA-Pro)
instead — that's where active development happens.

## Copyright / License

Copyright 2014 - 2019 Das Keyboard / Metadot Corp. (original code)  
Copyright 2026 fabioc-aloha (fork modifications)

Licensed under the GNU General Public License Version 2.0 (or later);
you may not use this work except in compliance with the License.
You may obtain a copy of the License in the LICENSE file, or at:

   <http://www.gnu.org/licenses/old-licenses/gpl-2.0.txt>

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
