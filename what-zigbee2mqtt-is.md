# What Zigbee2MQTT is

You finish this chapter knowing what problem
this bridge solves, and able to check
whether it is the right tool for your house:
if your devices speak Zigbee and you want
them on your own network with no vendor
account, it is.

Zigbee is a radio language for small, cheap,
battery-sipping devices: light bulbs, door
sensors, temperature sensors, smart plugs,
radiator valves. A single coin cell in a
door sensor can last a year because Zigbee
radios sleep almost all the time and whisper
when they wake.

The trouble is what normally sits between
those devices and you. Buy a bulb from one
maker and a sensor from another, and each
wants its own hub, its own app, its own
cloud account. When the maker loses interest
— or goes out of business — the app dies and
the hub becomes a paperweight, even though
the bulb is fine.

**Zigbee2MQTT** replaces all of that with
three things you run yourself:

- one USB radio stick (the **coordinator**)
plugged into a computer you own, - this
program, which talks to the stick and
translates every Zigbee device into plain
messages, - an **MQTT broker** — a simple
message post office — that those messages
flow through.

That is the whole trick. The bulb stops
being "a bulb in an app" and becomes a
stream of small text messages on your
network: `zigbee2mqtt/hallway_sensor` says
`{"contact": true}` when the door opens.
Anything that can read MQTT — a home
automation system, a script, a logger — can
use every device you own, from every maker,
in the same way. Nothing phones home. There
is no account to lose.

Home Assistant users meet Zigbee2MQTT most
often, and this guide uses it as the main
example of a consumer, but the bridge does
not belong to Home Assistant. The broker is
the meeting point, and this guide treats it
that way: get the bridge right, and the
automation system is the easy part.

The lab behind this guide ran the real
program — **Zigbee2MQTT 2.14.2**, the
official release — next to a real MQTT
broker (Mosquitto 2.0.18), with the same
configuration file this guide builds chapter
by chapter. One thing the lab did not have
is a Zigbee radio, so the story has a
deliberate cliff edge: chapter 6 shows the
bridge starting, greeting the broker, and
then failing honestly at the missing USB
stick, with the real log lines quoted. That
failure is worth reading before you buy
anything, because it is exactly what you
will see if your stick is unplugged, and the
guide could not honestly show you a paired
device it never had. What needs real
hardware — pairing, the device pages,
over-the-air updates — is taught from the
project's documentation and labelled as
such, here and in `criteria.md`.

## The check

You can run this check with nothing
installed. Ask three questions of the pile
of smart devices you own, or plan to own:

1. Do the boxes or listings say **Zigbee**
(not just "works with" some assistant)? If
yes, this bridge can probably run them — the
project's supported-devices list names
thousands of models, brand by brand. 2. Do
you have a small computer that stays on — a
Raspberry Pi, a mini PC, a NAS? The bridge
needs to be awake when a sensor whispers. 3.
Are you willing to run one more small
program, the MQTT broker, beside it? If you
already run one, the answer is trivially
yes.

Three yeses mean this guide's path fits you.
A no on the first question points you at the
neighbours in chapter 13 — ZHA, vendor hubs,
and the newer Thread and Matter world.
