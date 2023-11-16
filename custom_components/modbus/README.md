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


## Full Example for Dimplex Heatpump

```yaml
modbus:
  - name: ModbusHub
    type: tcp
    host: 10.0.0.60
    port: 502

    # Dimplex Heat Pump
    # Dimplex Modbus TCP documentation: https://dimplex.atlassian.net/wiki/spaces/DW/pages/3021504513/Modbus+TCP+Anbindung
    # Available state classes: https://developers.home-assistant.io/docs/core/entity/sensor/#available-state-classes
    # List of device classes: https://www.home-assistant.io/integrations/binary_sensor/
    sensors:
      # Betriebsdaten
      - name: Dimplex Wärmepumpe Außentemperatur (R1)
        device_address: 1
        address: 1 # Address for version H: 27
        input_type: holding
        data_type: int16
        scale: 0.1
        precision: 1
        unit_of_measurement: °C
      - name: Dimplex Wärmepumpe Heizung Rücklauftemperatur (R2) # 1. Heizkreis
        device_address: 1
        address: 2 # Address for version H: 29
        input_type: holding
        data_type: int16
        scale: 0.1
        precision: 1
        unit_of_measurement: °C
      - name: Dimplex Wärmepumpe Heizung Rücklaufsolltemperatur # 1. Heizkreis
        device_address: 1
        address: 53 # Address for version H: 28
        input_type: holding
        data_type: int16
        scale: 0.1
        precision: 1
        unit_of_measurement: °C
      - name: Dimplex Wärmepumpe Heizung Festwertsolltemperatur # 1. Heizkreis, kann auf andere Heizkreise konfiguriert werden
        device_address: 1
        address: 5037 # R/W, Address for version H: 5003
        input_type: holding
        data_type: int16
        scale: 1
        precision: 1
        unit_of_measurement: °C
      - name: Dimplex Wärmepumpe Warmwasser Temperatur (R3)
        device_address: 1
        address: 3 # Address for version H: 30
        input_type: holding
        data_type: int16
        scale: 0.1
        precision: 1
        unit_of_measurement: °C
      - name: Dimplex Wärmepumpe Warmwasser Solltemperatur
        device_address: 1
        address: 58 # Address for version H: 40
        input_type: holding
        data_type: int16
        scale: 0.1
        precision: 1
        unit_of_measurement: °C
      - name: Dimplex Wärmepumpe Warmwasser Festwertsolltemperatur
        device_address: 1
        address: 5047 # R/W, Address for version H: 5022
        input_type: holding
        data_type: int16
        scale: 1
        precision: 1
        unit_of_measurement: °C
      - name: Dimplex Wärmepumpe Vorlauftemperatur (R9)
        device_address: 1
        address: 5 # Address for version H: 31
        input_type: holding
        data_type: int16
        scale: 0.1
        precision: 1
        unit_of_measurement: °C
      - name: Dimplex Wärmepumpe Wärmequelleneintrittstemperatur (R24)
        device_address: 1
        address: 6
        input_type: holding
        data_type: int16
        scale: 0.1
        precision: 1
        unit_of_measurement: °C
      - name: Dimplex Wärmepumpe Wärmequellenaustrittstemperatur (R6)
        device_address: 1
        address: 7 # Address for version H: 41
        input_type: holding
        data_type: int16
        scale: 0.1
        precision: 1
        unit_of_measurement: °C
      # - name: Dimplex Wärmepumpe 2. Heizkreis Rücklauftemperatur (R5)
      #   device_address: 1
      #   address: 9 # Address for version H: 33
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C
      # - name: Dimplex Wärmepumpe 2. Heizkreis Rücklaufsolltemperatur
      #   device_address: 1
      #   address: 54 # Address for version H: 32
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C
      # - name: Dimplex Wärmepumpe 3. Heizkreis Rücklauftemperatur (R13)
      #   device_address: 1
      #   address: 10 # Address for version H: 35
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C
      # - name: Dimplex Wärmepumpe 3. Heizkreis Rücklaufsolltemperatur
      #   device_address: 1
      #   address: 55 # Address for version H: 34
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C
      # - name: Dimplex Wärmepumpe Raumtemperatur 1
      #   device_address: 1
      #   address: 11 # Address for version H: 36
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C
      # - name: Dimplex Wärmepumpe Raumtemperatur 2
      #   device_address: 1
      #   address: 12 # Address for version H: 38
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C
      # - name: Dimplex Wärmepumpe Raumfeuchte 1
      #   device_address: 1
      #   address: 13 # Address for version H: 37
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C
      # - name: Dimplex Wärmepumpe Raumfeuchte 2
      #   device_address: 1
      #   address: 14 # Address for version H: 39
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C

      ## Passiv Kühlen
      # - name: Dimplex Wärmepumpe Vorlauftemperatur (R11)
      #   device_address: 1
      #   address: 19 # Address for version H: 42
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C
      # - name: Dimplex Wärmepumpe Rücklauftemperatur (R4)
      #   device_address: 1
      #   address: 20 # Address for version H: 43
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C
      # - name: Dimplex Wärmepumpe Rücklauftemp. gem. Primärkreis (R24)
      #   device_address: 1
      #   address: 21
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C

      ## Solar
      # - name: Dimplex Wärmepumpe Kollektorfühler (R23)
      #   device_address: 1
      #   address: 10
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C
      # - name: Dimplex Wärmepumpe Solarspeicher (R22)
      #   device_address: 1
      #   address: 23
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C

      # Lüftung
      # - name: Dimplex Wärmepumpe Außenlufttemperatur
      #   device_address: 1
      #   address: 120
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C
      # - name: Dimplex Wärmepumpe Zulufttemperatur
      #   device_address: 1
      #   address: 121
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C
      # - name: Dimplex Wärmepumpe Ablufttemperatur
      #   device_address: 1
      #   address: 122
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C
      # - name: Dimplex Wärmepumpe Fortlufttemperatur
      #   device_address: 1
      #   address: 123
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: °C
      # - name: Dimplex Wärmepumpe Drehzahl Zuluftventilator
      #   device_address: 1
      #   address: 125
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: U/min
      # - name: Dimplex Wärmepumpe Drehzahl Abluftventilator
      #   device_address: 1
      #   address: 126
      #   input_type: holding
      #   data_type: int16
      #   scale: 0.1
      #   precision: 1
      #   unit_of_measurement: U/min

      # Historie
      - name: Dimplex Wärmepumpe Verdichter 1
        device_address: 1
        address: 72 # Address for version H: 64
        input_type: holding
        data_type: uint16
        scale: 1
        precision: 0
        unit_of_measurement: h
        state_class: total_increasing
      # - name: Dimplex Wärmepumpe Verdichter 2
      #   device_address: 1
      #   address: 73 # Address for version H: 65
      #   input_type: holding
      #   data_type: uint16
      #   scale: 1
      #   precision: 0
      #   unit_of_measurement: h
      #   state_class: total_increasing
      - name: Dimplex Wärmepumpe Primärpumpe / Ventilator (M11)
        device_address: 1
        address: 74 # Address for version H: 66
        input_type: holding
        data_type: uint16
        scale: 1
        precision: 0
        unit_of_measurement: h
        state_class: total_increasing
      # - name: Dimplex Wärmepumpe 2. Wärmeerzeuger (E10)
      #   device_address: 1
      #   address: 75 # Address for version H: 67
      #   input_type: holding
      #   data_type: uint16
      #   scale: 1
      #   precision: 0
      #   unit_of_measurement: h
      #   state_class: total_increasing
      - name: Dimplex Wärmepumpe Heizungspumpe (M13)
        device_address: 1
        address: 76 # Address for version H: 68
        input_type: holding
        data_type: uint16
        scale: 1
        precision: 0
        unit_of_measurement: h
        state_class: total_increasing
      - name: Dimplex Wärmepumpe Warmwasserpumpe (M18)
        device_address: 1
        address: 77 # Address for version H: 69
        input_type: holding
        data_type: uint16
        scale: 1
        precision: 0
        unit_of_measurement: h
        state_class: total_increasing
      # - name: Dimplex Wärmepumpe Flanschheizung (E9)
      #   device_address: 1
      #   address: 78 # Address for version H: 70
      #   input_type: holding
      #   data_type: uint16
      #   scale: 1
      #   precision: 0
      #   unit_of_measurement: h
      #   state_class: total_increasing
      # - name: Dimplex Wärmepumpe Schwimmbadpumpe (M19)
      #   device_address: 1
      #   address: 79 # Address for version H: 71
      #   input_type: holding
      #   data_type: uint16
      #   scale: 1
      #   precision: 0
      #   unit_of_measurement: h
      #   state_class: total_increasing
      - name: Dimplex Wärmepumpe Zusatzumwälzpumpe (M16)
        device_address: 1
        address: 71
        input_type: holding
        data_type: uint16
        scale: 1
        precision: 0
        unit_of_measurement: h
        state_class: total_increasing
      # Wärmemenge Heizen = (Wärmemenge Heizen 9-12 * 100000000) + (Wärmemenge Heizen 5-8 * 10000) + Wärmemenge Heizen 1-4
      - name: Dimplex Wärmepumpe Wärmemenge Heizung 1-4
        unique_id: dimplex_warmepumpe_warmemenge_heizung_1_4
        device_address: 1
        address: 5096 # Address for version H: 5101
        input_type: holding
        data_type: uint16
        scale: 1
        precision: 0
        unit_of_measurement: kWh
      - name: Dimplex Wärmepumpe Wärmemenge Heizung 5-8
        unique_id: dimplex_warmepumpe_warmemenge_heizung_5_8
        device_address: 1
        address: 5097 # Address for version H: 5102
        input_type: holding
        data_type: uint16
        scale: 10000
        precision: 0
        unit_of_measurement: kWh
      - name: Dimplex Wärmepumpe Wärmemenge Heizung 9-12
        unique_id: dimplex_warmepumpe_warmemenge_heizung_9_12
        device_address: 1
        address: 5098 # Address for version H: 5103
        input_type: holding
        data_type: uint16
        scale: 100000000
        precision: 0
        unit_of_measurement: kWh
      - name: Dimplex Wärmepumpe Wärmemenge Warmwasser 1-4
        unique_id: dimplex_warmepumpe_warmemenge_warmwasser_1_4
        device_address: 1
        address: 5099 # Address for version H: 5104
        input_type: holding
        data_type: uint16
        scale: 1
        precision: 0
        unit_of_measurement: kWh
      - name: Dimplex Wärmepumpe Wärmemenge Warmwasser 5-8
        unique_id: dimplex_warmepumpe_warmemenge_warmwasser_5_8
        device_address: 1
        address: 5100 # Address for version H: 5105
        input_type: holding
        data_type: uint16
        scale: 10000
        precision: 0
        unit_of_measurement: kWh
      - name: Dimplex Wärmepumpe Wärmemenge Warmwasser 9-12
        unique_id: dimplex_warmepumpe_warmemenge_warmwasser_9_12
        device_address: 1
        address: 5101 # Address for version H: 5106
        input_type: holding
        data_type: uint16
        scale: 100000000
        precision: 0
        unit_of_measurement: kWh
      # - name: Dimplex Wärmepumpe Wärmemenge Schwimmbad 1-4
      #   device_address: 1
      #   address: 5102 # Address for version H: 5107
      #   input_type: holding
      #   data_type: uint16
      #   scale: 1
      #   precision: 0
      #   unit_of_measurement: kWh
      # - name: Dimplex Wärmepumpe Wärmemenge Schwimmbad 5-8
      #   device_address: 1
      #   address: 5103 # Address for version H: 5108
      #   input_type: holding
      #   data_type: uint16
      #   scale: 1
      #   precision: 0
      #   unit_of_measurement: kWh
      # - name: Dimplex Wärmepumpe Wärmemenge Schwimmbad 9-12
      #   device_address: 1
      #   address: 5104 # Address for version H: 5109
      #   input_type: holding
      #   data_type: uint16
      #   scale: 1
      #   precision: 0
      #   unit_of_measurement: kWh

      # Statusmeldungen
      - name: Dimplex Wärmepumpe Statusmeldung Nr
        unique_id: dimplex_warmepumpe_statusmeldung_nr
        device_address: 1
        address: 103 # Address for version J: 43, Address for version H: 14
        input_type: holding
        data_type: uint16
        scale: 1
        precision: 0
      - name: Dimplex Wärmepumpe Sperrmeldung Nr
        unique_id: dimplex_warmepumpe_sperrmeldung_nr
        device_address: 1
        address: 104 # Address for version J: 59, Address for version H: 94
        input_type: holding
        data_type: uint16
        scale: 1
        precision: 0
      - name: Dimplex Wärmepumpe Störmeldung Nr
        unique_id: dimplex_warmepumpe_stormeldung_nr
        device_address: 1
        address: 105 # Address for version J: 42, Address for version H: 13
        input_type: holding
        data_type: uint16
        scale: 1
        precision: 0
      - name: Dimplex Wärmepumpe Sensorikmeldung Nr
        unique_id: dimplex_warmepumpe_sensorikmeldung_nr
        device_address: 1
        address: 106
        input_type: holding
        data_type: uint16
        scale: 1
        precision: 0

    # Eingänge und Ausgänge
    binary_sensors:
      - name: Dimplex Wärmepumpe Warmwasserthermostat
        device_address: 1
        address: 3 # Address for version H: 57
        input_type: coil
        # device_class: None
      # - name: Dimplex Wärmepumpe Schwimmbadthermostat
      #   device_address: 1
      #   address: 4 # Address for version H: 58
      #   input_type: coil
      #   # device_class: None
      - name: Dimplex Wärmepumpe EVU-Sperre
        device_address: 1
        address: 5 # Address for version H: 56
        input_type: coil
        # device_class: safety
      - name: Dimplex Wärmepumpe Sperre Extern
        device_address: 1
        address: 6 # Address for version H: 63
        input_type: coil
        # device_class: safety
      - name: Dimplex Wärmepumpe Verdichter 1
        device_address: 1
        address: 41 # Address for version H: 80
        input_type: coil
        device_class: running
      # - name: Dimplex Wärmepumpe Verdichter 2
      #   device_address: 1
      #   address: 42 # Address for version H: 81
      #   input_type: coil
      #   device_class: running
      - name: Dimplex Wärmepumpe Primärpumpe (M11) / Ventilator (M2)
        device_address: 1
        address: 43 # Address for version H: 82
        input_type: coil
        device_class: running
      # - name: Dimplex Wärmepumpe 2. Wärmeerzeuger (E10)
      #   device_address: 1
      #   address: 44 # Address for version H: 83
      #   input_type: coil
      #   # device_class: heat
      - name: Dimplex Wärmepumpe Heizungspumpe (M13)
        device_address: 1
        address: 45 # Address for version H: 84
        input_type: coil
        device_class: running
      - name: Dimplex Wärmepumpe Warmwasserpumpe (M18)
        device_address: 1
        address: 46 # Address for version H: 85
        input_type: coil
        device_class: running
      - name: Dimplex Wärmepumpe Mischer (M21) Auf
        device_address: 1
        address: 47 # Address for version H: 86
        input_type: coil
        device_class: opening
      # - name: Dimplex Wärmepumpe Mischer (M21) Zu
      #   device_address: 1
      #   address: 48 # Address for version H: 87
      #   input_type: coil
      #   # device_class: opening
      - name: Dimplex Wärmepumpe Zusatzumwälzpumpe (M16)
        device_address: 1
        address: 49 # Address for version H: 88
        input_type: coil
        device_class: running
      # - name: Dimplex Wärmepumpe Flanschheizung (E9)
      #   device_address: 1
      #   address: 50 # Address for version H: 89
      #   input_type: coil
      #   # device_class: heat
      - name: Dimplex Wärmepumpe Heizungspumpe (M15)
        device_address: 1
        address: 51 # Address for version H: 90
        input_type: coil
        device_class: running
      - name: Dimplex Wärmepumpe Mischer (M22) Auf
        device_address: 1
        address: 52 # Address for version H: 91
        input_type: coil
        device_class: opening
      # - name: Dimplex Wärmepumpe Mischer (M22) Zu
      #   device_address: 1
      #   address: 53 # Address for version H: 92
      #   input_type: coil
      #   # device_class: opening
      - name: Dimplex Wärmepumpe Schwimmbadpumpe (M19)
        device_address: 1
        address: 56 # Address for version H: 95
        input_type: coil
        device_class: running
      - name: Dimplex Wärmepumpe Sammelstörmeldung (H5)
        device_address: 1
        address: 57
        input_type: coil
        device_class: problem
      - name: Dimplex Wärmepumpe Heizungspumpe (M14)
        device_address: 1
        address: 59 # Address for version H: 94
        input_type: coil
        device_class: running
      - name: Dimplex Wärmepumpe Kühlpumpe (M17)
        device_address: 1
        address: 60 # Address for version H: 99
        input_type: coil
        device_class: running
      - name: Dimplex Wärmepumpe Heizungspumpe (M20)
        device_address: 1
        address: 61
        input_type: coil
        device_class: running
      - name: Dimplex Wärmepumpe Umschaltung Raumthermostate Heizen/Kühlen (N9)
        device_address: 1
        address: 66 # Address for version H: 96
        input_type: coil
        # device_class: hot
      - name: Dimplex Wärmepumpe Primärpumpe Kühlen (M12)
        device_address: 1
        address: 68 # Address for version H: 98
        input_type: coil
        device_class: running
      - name: Dimplex Wärmepumpe Solarpumpe (M23)
        device_address: 1
        address: 71
        input_type: coil
        device_class: running

    # Temperatursteuerung
    climates:
      # It should be configured like this:
      #   https://www.home-assistant.io/integrations/modbus/#example-climate-configuration
      - name: Dimplex Wärmepumpe Warmwasser
        device_address: 1
        address: 3 # Dimplex Wärmepumpe Temperatur Warmwasser (R3), Address for version H: 30
        target_temp_register: 5047 # Dimplex Wärmepumpe Festwertsolltemperatur Warmwasser, R/W, Address for version H: 5022
        target_temp_write_registers: false
        target_temp_scale: 1
        hvac_mode_register:
          address: 103
          values:
            # state_off: [0, 1, 2, 3, 24]
            state_off: [0, 1, 2, 3, 5, 24]
            state_heat: 4
            # state_cool: 5
        input_type: holding
        data_type: int16
        scale: 0.1
        precision: 1
        max_temp: 60
        min_temp: 15
        temp_step: 1
        temperature_unit: C
      - name: Dimplex Wärmepumpe Heizung # 1. Heizkreis
        device_address: 1
        address: 2 # Dimplex Wärmepumpe Temperatur Rücklauf (R2), Address for version H: 29
        target_temp_register: 5037 # Dimplex Wärmepumpe Festwertsolltemperatur Heizkreis, R/W, Address for version H: 5003
        target_temp_write_registers: false
        target_temp_scale: 1
        hvac_mode_register:
          address: 103
          values:
            # state_off: [0, 1, 3, 4, 24]
            state_off: [0, 1, 3, 4, 5, 24]
            state_heat: 2
            # state_cool: 5
        input_type: holding
        data_type: int16
        scale: 0.1
        precision: 1
        max_temp: 35
        min_temp: 15
        temp_step: 1
        temperature_unit: C

sensor:
  - platform: template
    sensors:
      dimplex_warmepumpe_statusmeldung:
        friendly_name: "Dimplex Wärmepumpe Statusmeldung"
        icon_template: mdi:information
        value_template: >-
          {% set mapper =  {
              '0': 'Aus',
              '1': 'Aus',
              '2': 'Heizen',
              '3': 'Schwimmbad',
              '4': 'Warmwasser',
              '5': 'Kühlen',
              '10': 'Abtauen',
              '11': 'Durchflussüberwachung',
              '24': 'Verzögerung Betriebsmodusumschaltung',
              '30': 'Sperre (siehe Wert für Sperren)' } %}
          {% set state =  states.sensor.dimplex_warmepumpe_statusmeldung_nr.state %}
          {{ mapper[state] if state in mapper else 'Unknown' }}

      dimplex_warmepumpe_sperrmeldung:
        friendly_name: "Dimplex Wärmepumpe Sperrmeldung"
        icon_template: mdi:information
        value_template: >-
          {% set mapper =  {
              '0': 'keine Meldung',
              '2': 'Volumenstrom',
              '5': 'Funktionskontrolle',
              '6': 'Einsatzgrenze HT',
              '7': 'Systemkontrolle',
              '8': 'Verzögerung Umschaltung Kühlen',
              '9': 'Pumpenvorlauf',
              '10': 'Mindeststandzeit',
              '11': 'Netzbelastung',
              '12': 'Schaltspielsperre',
              '13': 'Warmwasser Nacherwärmung',
              '14': 'Regenerativ',
              '15': 'EVU-Sperre',
              '16': 'Sanftanlasser',
              '17': 'Durchfluss',
              '18': 'Einsatzgrenze Wärmepumpe',
              '19': 'Hochdruck',
              '20': 'Niederdruck',
              '21': 'Einsatzgrenze Wärmequelle',
              '23': 'System Grenze',
              '24': 'Last Primärkreis',
              '25': 'Sperre Extern',
              '29': 'Inverter',
              '31': 'Aufwärmen',
              '33': 'EvD Initialisierung',
              '34': '2.Wärmeerzeuger freigegeben',
              '35': 'Störung (siehe Wert für Störmeldungen)' } %}
          {% set state =  states.sensor.dimplex_warmepumpe_sperrmeldung_nr.state %}
          {{ mapper[state] if state in mapper else 'Unknown' }}

      dimplex_warmepumpe_stoermeldung:
        friendly_name: "Dimplex Wärmepumpe Störmeldung"
        icon_template: mdi:information
        value_template: >-
          {% set mapper =  {
              '0': 'kein Fehler',
              '1': 'Fehler N17.1',
              '2': 'Fehler N17.2',
              '3': 'Fehler N17.3',
              '4': 'Fehler N17.4',
              '6': 'Elektronisches Ex.Ventil',
              '10': 'WPIO',
              '12': 'Inverter',
              '13': 'WQIF',
              '15': 'Sensorik',
              '16': 'Niederdruck Sole',
              '19': '!Primärkreis',
              '20': '!Abtauen',
              '21': '!Niederdruck Sole',
              '22': '!Warmwasser',
              '23': '!Last Verdichter',
              '24': '!Codierung',
              '25': '!Niederdruck',
              '26': '!Frostschutz',
              '28': '!Hochdruck',
              '29': '!Temperatur Differenz',
              '30': '!Heisgasthermostat',
              '31': '!Durchfluss',
              '32': '!Aufwärmen' } %}
          {% set state =  states.sensor.dimplex_warmepumpe_stormeldung_nr.state %}
          {{ mapper[state] if state in mapper else 'Unknown' }}

      dimplex_warmepumpe_sensorikmeldung:
        friendly_name: "Dimplex Wärmepumpe Sensorikmeldung"
        icon_template: mdi:information
        value_template: >-
          {% set mapper =  {
              '0': 'keine Meldung',
              '1': 'Außenfühler (R1)',
              '2': 'Rücklauffühler (R2)',
              '3': 'Warmwasserfühler (R3)',
              '4': 'Codierung (R7)',
              '5': 'Vorlauffühler (R9)',
              '6': '2.Heizkreisfühler (R5)',
              '7': '3.Heizkreisfühler (R13)',
              '8': 'Regenerativfühler (R13)',
              '9': 'Raumfühler 1',
              '10': 'Raumfühler 2',
              '11': 'Fühler Wärmequellenaustritt (R6)',
              '12': 'Fühler Wärmequelleneintritt (R24)*',
              '14': 'Kollektorfühler (R23)',
              '15': 'Niederdrucksensor (R25)',
              '16': 'Hochdrucksensor (R26)',
              '17': 'Raumfeuchte 1',
              '18': 'Raumfeuchte 2',
              '19': 'Fühler Frostschutz-Kälte',
              '20': 'Heißgas',
              '21': 'Rücklauffühler (R2.1)',
              '22': 'Schwimmbadfühler (R20)',
              '23': 'Vorlauffühler Kühlen Passiv (R11)',
              '24': 'Rücklauffühler Kühlen Passiv (R4)',
              '26': 'Fühler Solarspeicher (R22)',
              '28': 'Anforderungsfühler Heizen (R2.2)',
              '29': 'RTM Econ',
              '30': 'Anforderungsfühler Kühlen (R39)' } %}
          {% set state =  states.sensor.dimplex_warmepumpe_sensorikmeldung_nr.state %}
          {{ mapper[state] if state in mapper else 'Unknown' }}

      dimplex_warmepumpe_warmemenge_heizung:
        friendly_name: "Dimplex Wärmepumpe Wärmemenge Heizung"
        icon_template: mdi:meter-electric
        unit_of_measurement: kWh
        value_template: >-
          {{ states('sensor.dimplex_warmepumpe_warmemenge_heizung_1_4') | int +
            states('sensor.dimplex_warmepumpe_warmemenge_heizung_5_8') | int +
            states('sensor.dimplex_warmepumpe_warmemenge_heizung_9_12') | int }}

      dimplex_warmepumpe_warmemenge_warmwasser:
        friendly_name: "Dimplex Wärmepumpe Wärmemenge Warmwasser"
        icon_template: mdi:meter-electric
        unit_of_measurement: kWh
        value_template: >-
          {{ states('sensor.dimplex_warmepumpe_warmemenge_warmwasser_1_4') | int +
            states('sensor.dimplex_warmepumpe_warmemenge_warmwasser_5_8') | int +
            states('sensor.dimplex_warmepumpe_warmemenge_warmwasser_9_12') | int }}
```
