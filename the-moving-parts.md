# The moving parts

You finish this chapter able to name every
piece between a sensor and a screen, and
able to check your understanding with one
question per piece: if this part stopped,
what would I see?

Six parts do the work. Knowing which part
holds what turns later confusion into a
named problem.

## The devices

Bulbs, sensors, plugs, switches, valves —
made by many companies, speaking Zigbee over
the air. Mains-powered devices (plugs,
bulbs) also act as **routers**, relaying
messages for their neighbours and
strengthening the mesh. Battery devices
mostly sleep and never route — which is why
a network of only battery sensors can be
fragile, and a few smart plugs spread around
the house quietly hold it together.

*If a device stops:* one name goes quiet on
the broker; everything else carries on.

## The coordinator

One special radio — usually a USB stick —
that owns the network. It holds the
network's name, channel and security key,
admits new devices, and passes every message
between the air and the computer.
Zigbee2MQTT talks to it through a small
library called **zigbee-herdsman**, which
knows each adapter family's dialect:
`zstack` for Texas Instruments based sticks,
`ember` for Silicon Labs ones, plus
`deconz`, `zigate` and a few more.

*If the coordinator stops:* the whole
network is deaf. The bridge cannot start
without it — chapter 6 shows the exact log
lines, because the lab ran into precisely
this.

## The bridge — Zigbee2MQTT itself

The translator. It interviews each device
when it joins ("what are you, what can you
do?"), remembers the answers, and from then
on turns radio traffic into tidy JSON
messages, and turns commands back into radio
traffic. It also serves a built-in web
**frontend** — a dashboard where you can see
every device, flip a switch, rename things
and read the network map. The bridge keeps
its whole world in three files in its `data`
folder: the configuration, the list of known
devices, and a small database.

*If the bridge stops:* devices keep their
last state, automations go quiet, and the
broker shows the bridge itself marked
offline — a trick chapter 8 explains.

## The broker

MQTT is a post office, not a database.
Programs **publish** messages to named
addresses called **topics**, and other
programs **subscribe** to the topics they
care about. The broker (this guide uses
**Mosquitto**) holds no opinions: it takes a
message in and hands it to every subscriber.
A bridge message looks like this, and this
exact shape is verified in chapter 5 against
the lab's own broker:

```
zigbee2mqtt/hallway_sensor  {"contact":false,"battery":87}
```

*If the broker stops:* nothing moves. Bridge
and automation system both sit waiting, and
both reconnect by themselves when it
returns.

## The consumers

Anything subscribed to the topics: **Home
Assistant** (which can discover devices
automatically, chapter 9), a Node-RED flow,
a script of your own, a logger keeping
history. Consumers also publish commands
back — `zigbee2mqtt/kitchen_bulb/set` with
`{"state":"ON"}` — and the bridge turns
those into radio traffic.

*If a consumer stops:* the devices are
unaffected. The broker still carries every
message; that consumer simply misses them.

## The configuration file

One YAML file, `configuration.yaml`, tells
the bridge where the coordinator is plugged
in, where the broker lives, what password to
present, and which name (the **base topic**)
all its topics start with. Chapter 5 builds
the lab's real file line by line — the file
the bridge in chapter 6 actually read.

## The check

Cover the part names and answer from the
descriptions: which part interviews new
devices? Which part holds the network key?
Which part would a second automation system
talk to, without disturbing the first? (The
bridge; the coordinator; the broker.) If
those three feel obvious, you hold the model
the rest of the guide hangs on.
