# Modbus Integration Modification

## New Features
The following features have been added to the Modbus integration:

1. **Target Temperature Scale**: You can now define a different scale for the target temperature. For this use the config option `target_temp_scale`.

2. **Target Temperature Offset**: A different offset can be applied to the target temperature. For this use the config option `target_temp_offset`.

Both features are optional and can be configured per climate device in your Modbus setup. If no `target_temp_scale` or `target_temp_offset` is given, the `scale` and `offset` options are used.

## Configuration Changes
Below is an example of how to implement these new settings in your `configuration.yaml` file.

### Climate Configuration Example
```yaml
modbus:
  - name: hub1
    type: tcp
    host: 192.168.1.10
    port: 502
    climates:
      - name: Thermostat
        device_address: 1
        address: 3
        scale: 0.1
        target_temp_register: 300
        target_temp_scale: 1  # Optional: Define a different scale for target temperature
```

## Installation
To use this modified Modbus integration, you should follow these steps:

1. Copy the Modbus integration into `/config/custom_components/modbus`. You can use HACS for this. Add `https://github.com/dmatscheko/home-assistant` to the user defined repositories and install the `Modbus` integration.
2. Modify your `configuration.yaml` file to include the new `target_temp_scale` and/or `target_temp_offset` options, if needed.
3. Restart Home Assistant to apply the changes.

## Support
If you encounter any issues or require assistance with your Modbus configuration, please refer to the [Home Assistant Modbus documentation](https://www.home-assistant.io/integrations/modbus) or reach out at the [Issue Tracker](https://github.com/dmatscheko/home-assistant/issues).
