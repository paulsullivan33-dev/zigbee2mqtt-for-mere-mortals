# Topics, payloads and commands

You finish this chapter able to watch
your whole network's traffic with one
command, read any device's message at
sight, and switch a bulb from the
command line — the same publish and
subscribe moves the lab proved against
its broker in chapter 5, now with the
bridge's own naming system on them.

## The address system

Every topic starts with the base topic
from chapter 5 (`zigbee2mqtt`), then
the friendly name from chapter 7:

```
zigbee2mqtt/landing_motion
zigbee2mqtt/kitchen_bulb
zigbee2mqtt/bridge/state
```

State flows *out* on the bare topic.
Commands flow *in* on the same topic
with a suffix: **`/set`** to change
something, **`/get`** to ask for a fresh
reading. That is the entire addressing
scheme; nothing else needs inventing.

## What a state message looks like

A device publishes one JSON object with
everything it currently knows about
itself — not just the thing that changed:

```json
{"contact": false, "battery": 87,
 "temperature": 21.4, "linkquality": 132}
```

JSON is the point. A door sensor's
message carries its battery and the
radio's link quality alongside the
headline fact (`contact`), so a consumer
never has to ask twice, and a historian
logging these messages gets the whole
picture per event. Battery devices
publish when something happens or on
their own slow schedule; mains devices
can also be asked (a `/get`), and many
can be configured to report on a timer.

## Giving an order

Publishing to the `/set` topic changes
the device, and the bridge confirms by
publishing the new state on the bare
topic:

```
mosquitto_pub -t 'zigbee2mqtt/kitchen_bulb/set' \
    -m '{"state": "ON", "brightness": 180}'
```

Brightness runs 0–254, colour and colour
temperature take the forms each device's
page documents, and several properties
can ride in one message as above. A
command to a sleeping battery device is
held until it next wakes — the one case
where "I sent it" and "it happened" are
separated by the device's nap, not by
anything being broken.

## The bridge talks too

Topics under `zigbee2mqtt/bridge/` are
the bridge describing itself:

- **`bridge/state`** — `online` or
  `offline`, published as a *retained*
  message, and wired to MQTT's last-will
  machinery: if the bridge dies without
  saying goodbye, the broker itself marks
  it `offline`. A consumer can therefore
  always tell a silent network from a
  dead bridge — the distinction chapter 2
  promised.
- **`bridge/devices`** and
  **`bridge/info`** — the roster and the
  running configuration, for tools that
  want to build their own picture.
- **`bridge/log`** — the bridge's log
  republished as messages, if you turn
  it on; the frontend normally makes this
  unnecessary.

Requests follow a request/response
pattern on `bridge/request/…` topics —
permit join in chapter 7 was one — with
answers on the matching
`bridge/response/…` topic, including an
error field when the answer is no.

## Watching everything at once

MQTT topics are a tree, and `#` is the
wildcard for "everything below here".
The check below subscribes to the whole
bridge — the exact command shape the
lab verified against its own broker in
chapter 5, pointed now at real traffic:

```
mosquitto_sub -v -t 'zigbee2mqtt/#'
```

Leave it running while you walk the
house opening doors and flipping
switches. Each line names the room,
the device and its full state. It is
the fastest way to learn your own
network's voice, and the first tool to
reach for when an automation misbehaves:
was the message ever sent? This window
knows.
