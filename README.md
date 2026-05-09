# Maestro Test Flows

This folder contains Maestro flows for `com.sinognss.ws`.

## Entry Flows

- `flows/language_switch.yaml`: multi-language switching test, driven by `data/languages.csv`.
- `flows/connection_cycle.yaml`: connect/disconnect cycle test.
- `flows/rtk_collection.yaml`: RTK collection flow repeated three times.

The old root-level files are kept as compatibility wrappers:

- `test_language_switch.yaml`
- `chafen_flow.yaml`
- `scan.yaml`

## Reusable Subflows

- `subflows/open_language_settings.yaml`
- `subflows/confirm_language_change.yaml`
- `subflows/connect_and_disconnect_once.yaml`
- `subflows/collect_rtk_once.yaml`

Run examples:

```powershell
maestro test flows/language_switch.yaml
maestro test flows/connection_cycle.yaml
maestro test flows/rtk_collection.yaml
```
