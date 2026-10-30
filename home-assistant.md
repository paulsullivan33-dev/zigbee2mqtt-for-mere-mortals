# Home Assistant discovery

*This chapter is taught from the
project's documentation and labelled
documented-only in `criteria.md`: the
lab ran the bridge and the broker, but
no Home Assistant and no devices, so
there was nothing to discover.*

You finish this chapter with Home
Assistant finding your Zigbee devices by
itself — no YAML, no per-device setup —
and you will understand the one switch
that makes it happen and the messages
that carry it, so when an entity looks
wrong you know which side of the fence
to fix.

## The one switch

In chapter 5 the lab set:

```yaml
homeassistant:
  enabled: false
```

Setting it to `true` and restarting the
bridge turns on **MQTT discovery**. Home
Assistant must be running its MQTT
integration against the same broker —
the normal setup — and nothing else is
required. From then on, the two programs
divide the work the way chapter 2 drew
it: the bridge owns the devices, Home
Assistant owns the dashboards and
automations, and the broker introduces
them.

## What actually happens

For each device, the bridge publishes a
retained configuration message under the
discovery topic (by default
`homeassistant/…`), in effect saying:
"there is a thing called `landing_motion`;
it is a sensor; its state lives on
`zigbee2mqtt/landing_motion`; read
`contact` out of the JSON." Home
Assistant, subscribed to that prefix,
builds the entity on the spot — the
right kind (sensor, light, switch,
climate), with the right units and the
device's room for a name.

Because the discovery messages are
*retained*, the arrangement survives
restarts in any order: Home Assistant
can reboot and simply re-read the
post office's notice board. Because
they are republished when a device is
renamed, renaming in the frontend (the
name from chapter 7) renames in Home
Assistant too. And when a device is
removed from the bridge, its discovery
message is withdrawn and the entity
goes with it — the roster stays honest
without a spring clean.

## What you get per device

Everything the device **exposes** (the
project's word for a capability it
knows how to present) becomes part of
the entity set: a bulb is a light with
brightness and colour; a climate sensor
is three sensors; a plug is a switch
plus power metering where the hardware
measures it. Battery and link quality
ride along as diagnostic sensors, which
is how "the hallway sensor is at 12%"
finds you before the silence does.

Two presentation choices worth knowing
exist, both in the `homeassistant:`
block and both documented: the
discovery topic itself can be moved,
and there are switches for how action
events from remotes and switches are
surfaced, which matters when you build
automations on button presses.

## Where to fix what

The fence rule from chapter 2 pays off
here:

- **Device missing entirely** — the
  bridge's side. It is not paired, or
  discovery is off, or the two programs
  are looking at different brokers.
- **Entity present but wrong** — usually
  the bridge's device support (an
  interview that mapped a quirk badly);
  the device's page in the
  supported-devices list is the reference.
- **Entity right, automation wrong** —
  Home Assistant's side, and nothing in
  this guide's lab can help beyond
  pointing at the state topic with
  chapter 8's wildcard subscription to
  prove what the bridge actually said.

## The check

On your own system: pair one simple
device with discovery already on, then
open Home Assistant's device list. The
device is there, named as you named it,
before you have clicked anything. If
you renamed it in the frontend, the new
name follows within moments. That —
not a green dashboard — is discovery
working.
