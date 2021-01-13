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
  - platform: mqtt
    name: Cnc Controller Task Status
    state_topic: "<base topic>/task"
    value_template: "{{ value_json.action }}"
    json_attributes_topic: "<base topic>/sender"
    json_attributes_template: "{{ value_json.state | tojson }}"
```

### This would create 5 sensors that looks something like:

#### sensor.cnc_controller_state

**State:** `Idle` (current activity state)

**Attributes:**
```yaml
parserstate:
  feedrate: '0'
  modal:
    coolant: M9
    distance: G90
    feedrate: G94
    motion: G0
    plane: G17
    spindle: M5
    units: G21
    wcs: G54
  spindle: '0'
  tool: '0'
status:
  activeState: Alarm
  buf:
    planner: 14
    rx: 128
  feedrate: 0
  mpos:
    x: '0.000'
    'y': '0.000'
    z: '0.000'
  ov:
    - 100
    - 100
    - 100
  spindle: 0
  subState: 0
  wco:
    x: '-802.530'
    'y': '-731.260'
    z: '-67.782'
  wpos:
    x: '802.530'
    'y': '731.260'
    z: '67.782'
friendly_name: Cnc Controller State
```

#### sensor.cnc_controller_settings

**State:** `1.1f` (reported firmware version)

**Attributes:**
```yaml
$0: '10'
$1: '255'
$10: '255'
$100: '39.660'
$101: '40.120'
$102: '200.000'
$11: '0.020'
$110: '10000.000'
$111: '10000.000'
$112: '1000.000'
$12: '0.010'
$120: '500.000'
$121: '500.000'
$122: '270.000'
$13: '0'
$130: '849.000'
$131: '850.000'
$132: '95.000'
$2: '0'
$20: '1'
$21: '0'
$22: '1'
$23: '0'
$24: '100.000'
$25: '2000.000'
$26: '25'
$27: '3.000'
$3: '2'
$30: '1000'
$31: '0'
$32: '0'
$4: '0'
$5: '0'
$6: '0'
friendly_name: CNC Controller Settings
```

#### sensor.cnc_controller_instruction_queue

**State:** `0` (Queue size)

**Attributes:**
```yaml
changed: false
hold: false
holdReason: null
pending: false
queue: 0
friendly_name: CNC Controller Instruction Queue
```

#### sensor.cnc_controller_program_status

**State:** `my_gcode.nc` (Name of program)

**Attributes:**
```yaml
context: {}
elapsedTime: 0
finishTime: 0
hold: false
holdReason: null
name: 'my_gcode.nc'
received: 0
remainingTime: 0
sent: 0
size: 0
sp: 1
startTime: 0
total: 0
friendly_name: CNC Controller Program Status
```

#### sensor.cnc_controller_task_status

**State:** `started` (last action)

**Attributes:**
```yaml
action: started
state: No idea what goes in here TBH
friendly_name: CNC Controller Task Status
```