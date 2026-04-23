# Global Cache iTach IP2IR Multi

A Q-SYS Designer plugin that controls up to 20 Global Cache iTach IP2IR units (3 IR outputs each = 60 IR zones) from a single component. Built for A/V installations where one UCI needs to drive many IR-controlled TVs, soundbars, and AVRs.

Includes built-in profiles for common TVs and AVRs, plus IR learning directly from a real remote. Per-unit-per-port profile memory means each TV keeps its own brand setting. Staggered "All Power On/Off" handles mass power control without inrush.

## Installation

1. Download `GlobalCache_IP2IR.qplugx`
2. Drop it into your Q-SYS Designer plugins folder:
   - **Mac:** `~/Documents/QSC/Q-Sys Designer/Plugins/`
   - **Windows:** `Documents\QSC\Q-Sys Designer\Plugins\`
3. Restart Q-SYS Designer
4. The plugin appears under its category in the plugin list — drag it into your design

To update an existing plugin in a design, bump the `Version` in the plugin file. Q-SYS Designer will prompt to upgrade the component and wiring is preserved.

## Setup

1. Add the plugin to your design
2. In properties, set:
   - **Unit Count** — number of iTach units (1–20)
   - **Port 1 / 2 / 3 Name** — friendly names for your three IR outputs (e.g. "Main Bar", "Patio", "Soundbar"). These flow to the UI, dropdowns, and pin names.
3. Enter the IP of each unit in the plugin UI — auto-connects on entry
4. For each unit, select it and set the three port profiles (Samsung TV, LG TV, Sony TV, etc.) — these persist per unit

## IR Learning

If a TV brand isn't in the built-in profiles, or you need commands that aren't in the profile:

1. Select the unit that has the TV wired to it
2. Pick **Learn Port** (which of the 3 IR outputs)
3. Type a command name in **Learn Command** (use the canonical names so they match the remote button mapping: `power`, `power_on`, `power_off`, `vol_up`, `vol_down`, `mute`, `ch_up`, `ch_down`, `source`, `menu`, `guide`, `info`, `up`, `down`, `left`, `right`, `enter`, `exit`, `back`, `home`, `num_0` through `num_9`, `play`, `pause`, `stop`, `rew`, `fwd`, `record`, `red`, `green`, `yellow`, `blue`)
4. Press **Learn** — status shows "Listening"
5. Aim the remote at the iTach's front IR receiver and press the button
6. Captured code appears in **Learned Code**, status shows "Captured!"
7. Press **Save** — code is stored as JSON in the design file and persists across reboots

Learned codes override built-in profiles, so you can learn individual commands to fix quirky remotes.

## Shared Remote

The remote panel in the plugin routes commands to the **active unit** + **active port**:

1. Wire your UCI remote buttons (`Power`, `Vol Up`, `Mute`, `Input Source`, numbers, nav, etc.) to the plugin's remote controls
2. Select a unit, select an Active Port from the dropdown — next button press goes there

## Per-Unit Per-Port Control Pins

This is where the plugin shines for automation. Every IR output on every unit gets dedicated input pins:

- `U1-<PortName> Pwr` / `On` / `Off` / `Input`
- `U2-<PortName> Pwr` / `On` / `Off` / `Input`
- etc.

So with 10 units and port names "Bar", "Patio", "Soundbar", you get pins like `U3-Patio Off` — wire those to schedules, triggers, or other components to control any IR output directly.

## All Power On / Off (Staggered)

Built-in sequenced trigger for mass power control:

- Press **All Power On** — sends `power_on` to every port on every connected unit, staggered
- Press **All Power Off** — same but `power_off`
- Adjust **Stagger Delay** (50–2000ms) to control spacing — default 300ms

Both buttons also have input pins so you can trigger mass power from a schedule or a physical button wired elsewhere.

## Key Controls / Pins

| Pin | Direction | Purpose |
|---|---|---|
| `Unit IP 1..N` | Input | IP address per unit |
| `Unit Name 1..N` | Input | Friendly name per unit |
| `Unit Select 1..N` | Input | Select active unit |
| `Unit Status 1..N` | Output | Connection status |
| `Unit LED 1..N` | Output | Active unit LED |
| `Port 1 Power Off 1..N` etc. | Input | Per-unit-per-port direct control |
| `All Power On`, `All Power Off` | Input | Staggered mass control |
| `Stagger Delay` | Input | ms between staggered commands |
| `Power`, `Vol Up`, `Mute`, etc. | Input | Shared remote (routes to active unit/port) |

## Notes

- IR learning uses the iTach's `get_IRL` / `stop_IRL` protocol — the IR receiver on the front of the unit must see the remote
- Learned codes are shared across all units (same TV model = same code), only the port assignment changes which emitter fires
- Built-in profiles cover: Samsung TV, LG TV, Sony TV, Vizio TV, Hisense TV, Samsung Soundbar, Sony Soundbar, Denon AVR, Yamaha AVR


---

**VDA Solutions LLC** — [info@vdasolutionsny.com](mailto:info@vdasolutionsny.com)