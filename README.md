# HA Pico Blueprints

Reusable Home Assistant blueprints for Lutron Pico remotes.

## Included

- `blueprints/automation/chris/pico_room_controller.yaml`
  - Multi-remote support (many Picos can control the same lights)
  - Light target by entity, device, area, or group
  - ON/OFF immediate actions
  - UP/DOWN tap step + hold-repeat dimming
  - MIDDLE — **default**: short press recalls a **Scene**, long press **saves** current light states into that scene via `scene.create` + `snapshot_entities`; optional **Custom** mode uses your own short/long actions instead

## Install In Home Assistant

### One-click import (recommended)

[![Open your Home Assistant instance and import this blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/christcb03/ha-pico-blueprints/main/blueprints/automation/chris/pico_room_controller.yaml)

### Manual install

1. Copy the blueprint YAML into:
   - `config/blueprints/automation/chris/pico_room_controller.yaml`
2. In HA, go to `Settings -> Automations & Scenes -> Blueprints`.
3. Import/create an automation from this blueprint.

## Notes

- This is a first working draft intended for iterative testing.
- Double-press actions are planned next after validating hold/release behavior across real hardware.

## Typical Caseta 5-button Pico numbering (Lutron / HA)

Numbers are **fixed by the remote model + integration** — you usually map once per room.

| Button (physical) | Often reported as |
|-------------------|-------------------|
| Middle / favorite | 1 |
| On                | 2 |
| Raise             | 3 |
| Off               | 4 |
| Lower             | 5 |

Always confirm with **Developer Tools → Events** (`lutron_caseta_button_event`) or blueprint **Debug — notify every Pico event**.

## Persisting button numbers (optional helpers)

Automation instances **already save** their inputs in Home Assistant; you should not need to re-type digits unless you **delete and recreate** the automation or use **Take control** and wipe fields.

To reuse the **same** digits across **multiple** automations (or change them in one place), create five **Helpers → Number** (min **1**, max **10**, step **1**), set each to the Caseta digit, then in every Pico automation select them under **Optional — helper …** for On / Off / Raise / Lower / Middle.

## Troubleshooting

- **Down does nothing but up works:** Re-import the latest blueprint. Older drafts compared button numbers loosely (`"5" == 5` can fail in templates). v0.1 on GitHub now compares with `(btn | int) == (button_down | int)`.
- **See exactly what the remote sends:** In the automation/blueprint, enable **Debug — notify every Pico event**, then press each button. Use the `button=` value in the notification for your mapping. Turn debug off after.
- **Developer Tools:** Listen to event `lutron_caseta_button_event` and compare `press` vs `release` for the lower paddle.
- **Lights keep dimming after you let go / ON flashes then dims again:** v0.1.2 uses **restart** mode so a new button press cancels an in-progress UP/DOWN repeat loop. Re-import the blueprint and recreate or re-save the automation. Earlier **parallel** mode could leave an old “hold dim” loop running while a new press started another run.
- **Middle “does nothing”:** From v0.1.3+, **Middle behavior** defaults to **Favorite scene**. You must pick a **Favorite scene entity** (Scene helper) and at least one **`light.*`** under **Favorite snapshot — light entities** (areas/groups cannot be snapshotted). From v0.1.4, if something is missing, turn on **Notify if middle action is skipped** (default on) and you’ll get a persistent notification explaining which part is incomplete. Also enable **Debug — notify when middle short or long runs** to confirm short vs long timing. For custom behavior, set **Middle behavior** to **Custom**.
- **ON at 100%:** v0.1.2 turns lights on with `brightness_pct: 100` for the ON button path.
