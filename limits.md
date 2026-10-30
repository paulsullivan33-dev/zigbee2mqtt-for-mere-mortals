# Limits

You finish this chapter with the honest
list: what Zigbee2MQTT cannot do, what
it does only as well as your house
allows, and which of its sharp edges
the lab ran into itself. A tool whose
limits are named is a tool you can
plan around.

## It cannot start without the radio

The lab's founding fact (chapter 6):
no coordinator, no bridge. There is no
simulation mode, no dummy adapter in
the current release, no way to rehearse
the device chapters on a machine with
no stick. The coordinator is also a
single point of failure all by itself —
one stick, one network — so the plan
for a dead stick is a spare on the
shelf or a patient wait for the post,
not a failover.

## One bridge, one network

A coordinator can be driven by exactly
one program at a time, and a device
belongs to exactly one network. You
cannot run Zigbee2MQTT and ZHA against
the same stick "to compare", and
merging two households' networks is a
re-pairing exercise, not a merge. The
`data/` folder and the stick are a
married pair (recipe 1); treat them
that way.

## The mesh is physics, not software

The bridge cannot fix distance, metal,
or a house built like a bunker. A
network of battery sensors with no
mains-powered routers between them and
the coordinator will be exactly as
fragile as the radio says it is.
Zigbee2MQTT reports that fragility
honestly — link quality, last seen —
but the remedy is plugs and placement
(chapters 3 and 11), not settings.

## Device support is per-model, forever

"Zigbee" on the box means the radio
language, not full understanding. The
project supports thousands of models,
but each one's quirks were mapped by
somebody, and a brand-new or obscure
device may interview as *unsupported*,
or expose only part of itself, until
its converter is written or fixed. The
supported-devices list is therefore
part of the purchase (chapter 3), not
a formality — and a firmware update
(chapter 11) can move a device either
way across that line.

## Commands are not guarantees

A `/set` to an awake device is answered
in well under a second. The same
command to a sleeping battery device
waits for its next wake-up, which can
be minutes away — by design, because
the alternative is a flat battery.
Automations built on instant obedience
from sleepy sensors will feel haunted;
build them on *reports* instead.

## You are the cloud now

The flip side of "no vendor account":
no vendor is keeping this running
either. The broker's password, the
frontend's token if you expose it, the
network key in your backup, the bridge
computer's own updates — all yours.
None of it is heavy (this chapter's
neighbours in the series carry the
security basics), but it is not
nobody's job, and a bridge left
unpatched on an exposed port is a
choice, not an accident.

## The check

Name, for your planned house, the
answer to each: if the coordinator
dies tonight, what runs the lights
tomorrow? Which room is furthest,
through the worst walls, from your
intended stick — and what mains device
will live between them? Which device on
your shopping list is *not* yet on the
supported list? A guide that leaves you
with three answers has done more for
the network than any single setting.
