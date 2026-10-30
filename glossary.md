# Glossary

The terms, collected in one place. Each
one is defined where it first appears
in the guide; this page is the shortcut.

**Adapter** — the setting (and the
hardware dialect) naming the coordinator
family: `zstack`, `ember`, `deconz`,
`zigate`, `ezsp`, `zboss`, `zoh`.

**Base topic** — the prefix every topic
in a bridge's tree starts with;
`zigbee2mqtt` by default.

**Binding** — a Zigbee-level instruction
telling one device to speak directly to
another, set up by the bridge and then
independent of it.

**Bridge** — Zigbee2MQTT itself: the
translator between the radio mesh and
the broker.

**Broker** — the MQTT post office
(Mosquitto in this guide). Programs
publish to it and subscribe from it;
it keeps no state of its own about what
the messages mean.

**Cluster** — Zigbee's word for a bundle
of related features: on/off, level
control, temperature measurement.

**Coordinator** — the one radio that
owns the network: usually a USB stick,
holding the channel and the network key.

**Discovery** — Home Assistant's
arrangement by which the bridge
announces each device as a retained
message and entities build themselves.

**Exposes** — the project's word for the
capabilities of a device it knows how
to present: a bulb exposes brightness,
a sensor exposes `contact` and
`battery`.

**Friendly name** — the human name given
to a device at pairing; it becomes the
device's topic, so it is an address,
not a label.

**Frontend** — the bridge's built-in web
dashboard, on port 8080 by default.

**Group** — a named set of receiving
devices addressed as one topic.

**Interview** — the bridge's first
conversation with a joined device,
establishing what it is and what it
exposes.

**Last will** — the MQTT arrangement by
which the broker itself marks a client
that vanished: how `bridge/state`
becomes `offline` when the bridge dies.

**Link quality** — a 0–255 grade of the
last radio hop a device used; a trend
to watch, not a verdict.

**MQTT** — the small messaging protocol
at the centre of the guide: publish,
subscribe, topics.

**Network key** — the shared secret the
whole mesh encrypts with; generated on
first start, kept in
`configuration.yaml`, protected like a
house key.

**PAN ID** — the network's numeric
identity on the air (with its extended
form, `ext_pan_id`).

**Permit join** — the timed window in
which the network admits new devices.
Open it to pair; let it close behind
you.

**Retained message** — a message the
broker keeps and hands to every new
subscriber: how discovery and
`bridge/state` survive restarts.

**Router** — in Zigbee, a mains-powered
device that also relays other devices'
messages. Not the box from your ISP.

**Scene** — a remembered arrangement of
states inside a group, stored in the
devices and recalled by number.

**Topic** — an MQTT address:
`zigbee2mqtt/kitchen_bulb`, with `/set`
and `/get` suffixes for commands.

**zigbee-herdsman** — the library inside
Zigbee2MQTT that speaks each adapter
family's dialect to the coordinator.
