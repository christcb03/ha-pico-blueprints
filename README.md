# HA Pico Blueprints

Reusable Home Assistant blueprints for Lutron Pico remotes.

## Included

- `blueprints/automation/chris/pico_room_controller.yaml`
  - Multi-remote support (many Picos can control the same lights)
  - Light target by entity, device, area, or group
  - ON/OFF immediate actions
  - UP/DOWN tap step + hold-repeat dimming
  - MIDDLE short-hold and long-hold actions

## Install In Home Assistant

1. Copy the blueprint YAML into:
   - `config/blueprints/automation/chris/pico_room_controller.yaml`
2. In HA, go to `Settings -> Automations & Scenes -> Blueprints`.
3. Import/create an automation from this blueprint.

## Notes

- This is a first working draft intended for iterative testing.
- Double-press actions are planned next after validating hold/release behavior across real hardware.
