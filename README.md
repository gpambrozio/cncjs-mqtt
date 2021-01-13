# cncjs-pendant-mqtt
Connect cncjs to an mqtt pendant. This could be used to connect it to your home automation system like home assistant. Useful for adding monitoring dashboards or automation.

## Installation
```sh
$ npm install
```

## Running

```sh
$ ./bin/cncjs-mqtt \
      --cncjs-address 'localhost' \
      --cncjs-port '8000' \
      --secret 'your secret from .cncrc' \
      --mqtt-address 'mqtt.server.local' \
      --mqtt-username 'cncjs mqtt username' \
      --mqtt-password 'secret stuff' \
      --mqtt-port '1883' \
      --port '/path/to/port'
```

## Integration with home Assistant

Since discovery isn't implemented yet, you'll have to manually implement the sensors in your `configuration.yaml`.

```yaml
sensor:
  - platform: mqtt
    name: Cnc Controller State
    state_topic: "<base topic>/state"
    value_template: "{{ value_json.status.activeState }}"
    json_attributes_topic: "<base topic>/state"
    json_attributes_template: "{{ value_json | tojson }}"
  - platform: mqtt
    name: Cnc Controller Settings
    state_topic: "<base topic>/settings"
    value_template: "{{ value_json.version }}"
    json_attributes_topic: "<base topic>/settings"
    json_attributes_template: "{{ value_json.settings | tojson }}"
  - platform: mqtt
    name: Cnc Controller Instruction Queue
    state_topic: "<base topic>/feeder"
    value_template: "{{ value_json.queue }}"
    json_attributes_topic: "<base topic>/feeder"
    json_attributes_template: "{{ value_json | tojson }}"
  - platform: mqtt
    name: Cnc Controller Program Status
    state_topic: "<base topic>/sender"
    value_template: "{{ value_json.name }}"
    json_attributes_topic: "<base topic>/sender"
    json_attributes_template: "{{ value_json | tojson }}"
```
