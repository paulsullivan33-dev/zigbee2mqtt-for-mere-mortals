# Zigbee2MQTT for Mere Mortals

Every brand of smart bulb wants its
own hub, its own app and its own cloud
account — and the bulb outlives the
company's interest in all three.
**Zigbee2MQTT** does the same job with
the brands left out. One USB radio
stick in a computer you own, one
bridge program, and one small message
broker replace the shelf of hubs:
every Zigbee device you own, from
every maker, appears on your own
network as plain text messages that
any of your other programs can read.
No accounts. Nothing phones home. When
a sensor whispers, the message crosses
*your* network and stops there.

This guide builds that bridge from its
parts: what the pieces are, which
radio stick to buy, how to install the
program, how to give the broker a
password and mean it, and how the
configuration file fits together. Then
it does the thing this series does: it
starts the real program and reads what
happens — including the part where the
story stops, because a bridge is only
half the machine and the lab had no
radio for it.

The lab behind this guide ran the real
thing: **Zigbee2MQTT 2.14.2**, the
official release, installed from its
own archive and compiled on the lab
machine, next to a real **Mosquitto
2.0.18** broker with password
authentication switched on. Against
that pair the lab proved the broker
with the bridge's own credentials (and
proved it refuses the wrong ones),
wrote the configuration file this
guide teaches, and started the bridge
— twice. Once with the configuration
pointing at a stick that does not
exist, and once at a software stand-in
for a serial port. Both starts failed,
honestly and informatively, in under
three seconds each, and chapter 6
quotes the logs in full: how far a
startup gets turns out to be the best
diagnostic the program offers, and a
reader who has seen these two failures
will recognise their own in seconds.

What the lab could not do, it did not
fake. It had no coordinator and no
Zigbee devices, so pairing a device,
watching it interview, driving it from
Home Assistant, the network map and
over-the-air updates are taught from
the project's documentation and
labelled as such — at the head of each
affected chapter and in the ledger in
`criteria.md`. The bridge, the broker,
the configuration and the failure
modes are not documentation. They are
transcripts.

## What this guide covers

- [**What Zigbee2MQTT is**](what-zigbee2mqtt-is.md) —
  the model: devices become messages
  on your own network, and a
  three-question check for whether
  this is your tool
- [**The moving parts**](the-moving-parts.md) —
  devices, coordinator, bridge,
  broker, consumers and the
  configuration file, and what each
  one's silence would mean
- [**Choosing a coordinator**](choosing-a-coordinator.md) —
  the chip is the product: CC2652 or
  EFR32, firmware you can update, and
  placement that costs nothing
- [**Installing Zigbee2MQTT**](installing.md) —
  the standalone install from the
  official release, the two pnpm
  commands that do the work, and the
  version checks that prove it
- [**The broker and the configuration file**](the-broker-and-config.md) —
  Mosquitto with a password, proof the
  bridge's credentials work, and the
  real `configuration.yaml` line by
  line
- [**First start — and the missing radio**](first-start.md) —
  two genuine failed starts read line
  by line, what a healthy start looks
  like, and why the broker is not in
  the early log
- [**Pairing your first device**](pairing.md) —
  permit join, the interview, naming
  devices like addresses, and the
  three stalls and what each means
- [**Topics, payloads and commands**](topics-and-commands.md) —
  the address system, JSON state,
  `/set` and `/get`, the bridge's own
  topics, and watching the whole
  network with one subscription
- [**Home Assistant discovery**](home-assistant.md) —
  the one switch that makes devices
  appear by themselves, and which
  side of the fence to fix when they
  look wrong
- [**Groups, scenes and bindings**](groups-and-bindings.md) —
  three ways devices act together,
  and the one that keeps working with
  every computer switched off
- [**The map, the channel and over-the-air updates**](network-and-updates.md) —
  reading link quality, why channel
  changes are planned operations,
  and the two kinds of firmware
- [**Recipes**](recipes.md) — back up
  the whole network in one copy, a
  watchdog for a dead bridge, the
  evening walk, a light that must
  never fail, and moving house for
  the bridge itself
- [**The neighbours**](the-neighbours.md) —
  ZHA, deCONZ, vendor hubs, and
  Thread and Matter: what each does
  that this bridge doesn't, and vice
  versa
- [**Limits**](limits.md) — no radio,
  no start; one stick, one network;
  the mesh is physics; and you are
  the cloud now
- [**Troubleshooting**](troubleshooting.md) —
  the missing port, the port that is
  not a radio, the password changed
  in one file only, the silent
  device, and the sluggish network
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Thread and Matter devices at any
depth — chapter 13 places them, and
they are another guide's subject.
Writing your own device converters for
unsupported hardware is pointed at,
through the project's documentation,
not taught. Home Assistant itself —
automations, dashboards, its own
care — has its own guide in this
series; this one stops at the fence
between the bridge and its consumers,
having made sure everything crossing
the fence is visible.

## How to use this guide

Read chapter 1 for the idea and
chapter 2 for the parts; chapter 3
before you spend money. Chapters 4 to
6 get a real bridge installed, checked
and started, and chapter 6 is the one
to read slowly even if your own start
succeeds: its two failures are the
vocabulary your log will speak for the
life of the network. Chapters 7 to 11
then assume a radio is in your hand.
The lab's transcripts live in
`zigbee2mqtt-lab/transcripts/`, and
the configuration file among them is
the exact file the lab's bridge read.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [MQTT for Mere Mortals](https://paulsullivan33-dev.github.io/mqtt-for-mere-mortals/)
- [Home Assistant for Mere Mortals](https://paulsullivan33-dev.github.io/home-assistant-for-mere-mortals/)
- [ESPHome for Mere Mortals](https://paulsullivan33-dev.github.io/esphome-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/zigbee2mqtt-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/zigbee2mqtt-for-mere-mortals/blob/main/LICENSE-CODE).
