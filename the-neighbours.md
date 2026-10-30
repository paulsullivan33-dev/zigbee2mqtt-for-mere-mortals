# The neighbours

You finish this chapter able to place
Zigbee2MQTT among its alternatives in
one sentence each — including the honest
cases where a neighbour is the better
choice. These characterisations are
from the projects' own documentation;
none of the neighbours was run in this
guide's lab.

## ZHA — the built-in

Home Assistant ships its own Zigbee
integration, **ZHA**, speaking to the
same coordinators through much of the
same low-level code. ZHA needs no
broker and no second program, appears
the moment you plug a stick in, and
covers the mainstream devices well. The
trade is reach and independence:
Zigbee2MQTT's device support is broader
(it is where many new devices land
first), its frontend shows the network
in more detail, and — the structural
point of this whole guide — its devices
live on a broker, so anything else can
use them on the day Home Assistant is
being rebuilt. If you will only ever
run Home Assistant and your devices are
mainstream, ZHA is a perfectly good
answer; this guide exists for the other
answer.

## deCONZ and the Dresden Elektronik world

The ConBee and RaspBee coordinators
come with their own software, **deCONZ**,
and a friendly front end, Phoscon. It is
a complete, polished ecosystem of its
own — and a separate silo: its devices
reach your other systems through its
own API rather than a shared broker.
Zigbee2MQTT can also drive those same
sticks (the `deconz` adapter of
chapter 3), which is the polite way to
migrate out.

## Vendor hubs

A Hue Bridge or its cousins still do
some things beautifully: entertainment
lighting effects, the vendor's own
scenes and switches, firmware updates
delivered without thought. Their cost
is the one chapter 1 opened with — an
account, an app, a cloud dependency,
and a hard border at the brand's edge.
Many households keep one hub for the
bulbs it handles best and this bridge
for everything else; the two networks
coexist as long as their channels are
planned not to sit on each other.

## Thread and Matter — the newer road

**Thread** is a mesh radio in the same
spirit as Zigbee, and **Matter** is the
application language that runs over it
(and over ordinary Wi-Fi). The promise
is the one this guide is built on — one
network, many makers — negotiated by
the industry instead of by a bridge
program. It is arriving rather than
arrived: device choice is still thinner
than Zigbee's, and the tooling is
younger. Nothing in this guide becomes
wasted when a Matter device joins the
house — the broker pattern is the same,
and Home Assistant speaks both — but
for a house being equipped today,
Zigbee's catalogue is the deeper one.

## The check

One sentence per neighbour, from the
descriptions: which alternative needs
no broker at all? Which would you keep
for entertainment lighting? Which new
road promises this guide's whole idea
as an industry standard? (ZHA; a vendor
hub; Matter over Thread.)
