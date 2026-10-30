# Choosing a coordinator

You finish this chapter knowing what to buy,
what to avoid, and where to plug it in. The
check is a shopping-list test: every stick
on it names its radio chip, because the chip
— not the brand on the plastic — decides
whether Zigbee2MQTT can drive it.

The lab owned no coordinator, so this
chapter is the one honest piece of shopping
advice in the guide: it is assembled from
the project's own supported-adapters list,
and `criteria.md` marks it documented-only.
What the lab *could* prove is what happens
with no stick at all (chapter 6) — buy
nothing until you have seen that failure and
know it is not yours.

## The chip is the product

Zigbee2MQTT drives the stick through
zigbee-herdsman, and each chip family speaks
a different dialect (the `adapter` setting
in chapter 5). The families that matter for
a new purchase:

- **Texas Instruments CC2652** (in its P/RB
variants) — `adapter: zstack`. The
long-standing recommendation: strong radio,
mature firmware the project itself
publishes, generous device limits. Sold in
sticks such as the SONOFF ZBDongle-P and in
the SMLIGHT SLZB family. - **Silicon Labs
EFR32MG21** — `adapter: ember`. The other
first-class choice, found in the SONOFF
ZBDongle-E and Home Assistant's Connect
ZBT-1. Firmware comes from the stick's maker
rather than the project, which is the main
practical difference. - **Older chips to
decline as gifts:** the CC2530 and CC2531
sticks that fill bargain bins. They work,
and the project still supports them, but
their small radios and low device ceilings
make them the first thing you will replace.
A network is tedious to move, so start on a
chip you can keep.

Ethernet coordinators exist too — the SLZB
line can sit on the network instead of a USB
port, which suits a bridge running in a
cupboard far from the server. The
configuration difference is one line: the
`port` becomes a `tcp://` address instead of
a device path (chapter 5).

## Firmware is part of the purchase

A coordinator runs its own little operating
system. The stick arrives with *some*
version; the project's adapter pages name
the firmware versions it is tested against,
and updating a zstack stick is a documented
ten-minute job with the project's own
firmware files and a small flashing tool.
You do not need to flash on day one —
chapter 11 returns to updates — but buy a
stick whose firmware *can* be updated by
you, not one sealed behind a vendor's app.

## Placement is radio work, and it is free

Zigbee lives at 2.4 GHz, sharing the air
with Wi-Fi and with the electrical noise of
USB 3 ports and SSDs, both of which are
famous for spraying interference at exactly
this frequency. Three habits, all free:

- **Use the short USB extension cable** that
comes in most stick boxes. It moves the
radio out of the computer's noise and out
from behind a metal case. - **Put the
coordinator near the middle of the house**,
not in the corner where the server happens
to live. Mains-powered bulbs and plugs relay
for it. - **Mind the channel.** Zigbee
channels 11–25 overlap Wi-Fi channels 1, 6
and 11 unevenly; the project's default
channel is 11, and channel 25 sits furthest
from the usual Wi-Fi crowd. Changing the
channel later makes battery devices rejoin,
so it is worth one thought now and none
afterwards.

## The check

Read your shortlist back against three
questions. Does the listing name the chip
(CC2652 or EFR32), not just the brand? Does
the maker's or the project's page show a
firmware update path? Do you have an
extension cable and a central-ish USB port
for it? Three yeses — that is the stick. The
bridge will find it at the other end of
chapter 5's `serial:` block.
